# springbucks2

> Spring MVC coffee shop service with HandlerInterceptor performance monitoring and comprehensive RESTful API

[![Java](https://img.shields.io/badge/Java-21-orange.svg)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.5-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Spring Data JPA](https://img.shields.io/badge/Spring%20Data%20JPA-3.4-blue.svg)](https://spring.io/projects/spring-data-jpa)
[![H2 Database](https://img.shields.io/badge/H2%20Database-2.2.224-yellow.svg)](https://www.h2database.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A comprehensive demonstration of using **Spring MVC** with custom **HandlerInterceptor** for performance monitoring, featuring coffee shop management system, RESTful API design, JPA data persistence, caching mechanism, and request performance tracking.

## Features

- Spring MVC with HandlerInterceptor (NOT simple Controller)
- Performance monitoring with custom interceptor
- Request lifecycle tracking (preHandle, postHandle, afterCompletion)
- ThreadLocal-based request timing
- Coffee product management and order processing
- RESTful API with comprehensive endpoints
- JPA/Hibernate data persistence
- Spring Cache mechanism for performance optimization
- Money type serialization/deserialization (TWD currency)
- Bean Validation for data integrity

## Tech Stack

### Core Frameworks
- **Spring Boot 3.4.5** - Microservices framework
- **Spring MVC** - Web application framework
- **Spring Data JPA** - Data persistence layer
- **Hibernate 6.x** - ORM framework

### Database & Tools
- **H2 Database** - In-memory database
- **Joda Money 2.0.2** - Money handling
- **Jackson** - JSON/XML serialization
- **Bean Validation** - Data validation

### Development Tools & Libraries
- **Lombok** - Reduce boilerplate code
- **Apache Commons Lang3** - Utility library
- **Maven 3.8+** - Build tool
- **Java 21** - Development environment

## Project Structure

```
springbucks2/
├── src/
│   ├── main/
│   │   ├── java/tw/fengqing/spring/springbucks/waiter/
│   │   │   ├── WaiterServiceApplication.java      # Main application
│   │   │   ├── controller/          # Controller layer
│   │   │   │   ├── CoffeeController.java
│   │   │   │   ├── CoffeeOrderController.java
│   │   │   │   ├── PerformanceInteceptor.java     # Performance monitoring (KEY)
│   │   │   │   └── request/         # Request objects
│   │   │   │       ├── NewCoffeeRequest.java
│   │   │   │       └── NewOrderRequest.java
│   │   │   ├── service/             # Service layer
│   │   │   │   ├── CoffeeService.java
│   │   │   │   └── CoffeeOrderService.java
│   │   │   ├── repository/          # Data access layer
│   │   │   │   ├── CoffeeRepository.java
│   │   │   │   └── CoffeeOrderRepository.java
│   │   │   ├── model/               # Entity models
│   │   │   │   ├── Coffee.java
│   │   │   │   ├── CoffeeOrder.java
│   │   │   │   ├── BaseEntity.java
│   │   │   │   ├── OrderState.java
│   │   │   │   └── MoneyConverter.java          # JPA Money converter
│   │   │   └── support/             # Support utilities
│   │   │       ├── MoneySerializer.java
│   │   │       ├── MoneyDeserializer.java
│   │   │       └── MoneyFormatter.java
│   │   └── resources/
│   │       ├── application.properties            # Application config
│   │       ├── schema.sql                        # Database schema
│   │       ├── data.sql                          # Initial data
│   │       └── coffee.txt                        # Coffee data file
│   └── test/
└── pom.xml
```

## Getting Started

### Prerequisites

- **Java 21** - Development environment
- **Maven 3.8+** - Build tool
- **IDE** - IntelliJ IDEA or Eclipse

### Installation & Execution

**Step 1: Clone the repository**

```bash
git clone <repository-url>
cd springbucks2
```

**Step 2: Compile the project**

```bash
mvn clean compile
```

**Step 3: Run the application**

```bash
mvn spring-boot:run
```

**Step 4: Verify service**

```bash
# Test coffee endpoint
curl http://localhost:8080/coffee/1

# Expected: Coffee JSON response with performance log
```

## Configuration

### Application Properties

```properties
# JPA configuration
spring.jpa.hibernate.ddl-auto=none
spring.jpa.properties.hibernate.show_sql=true
spring.jpa.properties.hibernate.format_sql=true

# Error message display (development environment)
server.error.include-message=always
server.error.include-binding-errors=always

# Server configuration
server.port=8080

# Jackson configuration
spring.jackson.time-zone=Asia/Taipei
```

**Configuration Details:**
- `ddl-auto=none`: Database schema managed by schema.sql
- `show_sql=true`: Display SQL statements for debugging
- `format_sql=true`: Format SQL output
- Error messages enabled for development (disable in production)

## HandlerInterceptor Deep Dive

### What is HandlerInterceptor?

`HandlerInterceptor` is a Spring MVC component that allows you to intercept HTTP requests and perform pre-processing and post-processing operations.

**Three Key Methods:**
```java
public interface HandlerInterceptor {
    // 1. Before controller method execution
    boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler);
    
    // 2. After controller method execution, before view rendering
    void postHandle(HttpServletRequest request, HttpServletResponse response, 
                   Object handler, ModelAndView modelAndView);
    
    // 3. After entire request completion (including view rendering)
    void afterCompletion(HttpServletRequest request, HttpServletResponse response, 
                        Object handler, Exception ex);
}
```

### Interceptor Registration

```java
@SpringBootApplication
@EnableJpaRepositories
@EnableCaching
public class WaiterServiceApplication implements WebMvcConfigurer {

    public static void main(String[] args) {
        SpringApplication.run(WaiterServiceApplication.class, args);
    }

    /**
     * Register custom interceptor
     * Apply to all /coffee/* and /order/* endpoints
     */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new PerformanceInteceptor())
                .addPathPatterns("/coffee/**")      // Apply to coffee endpoints
                .addPathPatterns("/order/**");      // Apply to order endpoints
    }
}
```

### PerformanceInteceptor Implementation

```java
@Slf4j
public class PerformanceInteceptor implements HandlerInterceptor {
    private ThreadLocal<StopWatch> stopWatch = new ThreadLocal<>();

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, 
                            Object handler) throws Exception {
        // Record request start time
        StopWatch sw = new StopWatch();
        stopWatch.set(sw);
        sw.start();
        return true; // true: continue, false: terminate request
    }
    
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, 
                          Object handler, ModelAndView modelAndView) throws Exception {
        // Record controller completion time
        stopWatch.get().stop();
        stopWatch.get().start();
    }
    
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, 
                               Object handler, Exception ex) throws Exception {
        // Calculate total time and log performance metrics
        StopWatch sw = stopWatch.get();
        sw.stop();
        
        String method = handler.getClass().getSimpleName();
        if (handler instanceof HandlerMethod) {
            String beanType = ((HandlerMethod) handler).getBeanType().getName();
            String methodName = ((HandlerMethod) handler).getMethod().getName();
            method = beanType + "." + methodName;
        }
        
        long totalTime = sw.getTotalTimeMillis();
        long lastTaskTime = 0;
        if (sw.getTaskCount() > 0) {
            StopWatch.TaskInfo[] taskInfos = sw.getTaskInfo();
            lastTaskTime = taskInfos[taskInfos.length - 1].getTimeMillis();
        }
        long processingTime = totalTime - lastTaskTime;
        
        log.info("{};{};{};{};{}ms;{}ms;{}ms", 
                request.getRequestURI(),
                method,
                response.getStatus(),
                ex == null ? "-" : ex.getClass().getSimpleName(),
                totalTime,
                processingTime,
                lastTaskTime);
        
        stopWatch.remove(); // Clean up ThreadLocal to prevent memory leak
    }
}
```

### Performance Log Analysis

**Sample Log Output:**
```log
2025-10-28T10:40:36.538+08:00  INFO 72109 --- [nio-8080-exec-2] t.f.s.s.w.c.PerformanceInteceptor : 
/coffee/1;tw.fengqing.spring.springbucks.waiter.controller.CoffeeController.getById;200;-;86ms;86ms;0ms
```

**Log Field Explanation:**

| Field | Value | Description |
|-------|-------|-------------|
| `/coffee/1` | Request URI | The requested endpoint |
| `CoffeeController.getById` | Handler method | Executed controller method |
| `200` | HTTP status | Response status code |
| `-` | Exception | Exception class name (or `-` if none) |
| `86ms` | Total time | Total request processing time |
| `86ms` | Processing time | Controller execution time |
| `0ms` | Rendering time | View rendering time (0 for REST API) |

### Interceptor Execution Flow

```
HTTP Request
    ↓
1. preHandle() - Start timer
    ↓
2. Controller execution (e.g., CoffeeController.getById)
    ↓
3. postHandle() - Record controller completion
    ↓
4. View rendering (if any)
    ↓
5. afterCompletion() - Log performance metrics & cleanup
    ↓
HTTP Response
```

### Key Implementation Details

**1. ThreadLocal Usage**
```java
private ThreadLocal<StopWatch> stopWatch = new ThreadLocal<>();
```
- Each thread has its own StopWatch instance
- Prevents race conditions in concurrent requests
- Must call `remove()` to prevent memory leaks

**2. StopWatch Timing**
```java
sw.start();  // Start timing
sw.stop();   // Stop timing
sw.getTotalTimeMillis();  // Get elapsed time
```

**3. Handler Type Detection**
```java
if (handler instanceof HandlerMethod) {
    // Extract controller class and method information
}
```

## API Usage Guide

### Coffee Management

**Get all coffees:**
```bash
curl http://localhost:8080/coffee/
```

**Get specific coffee:**
```bash
curl http://localhost:8080/coffee/1
```

**Response:**
```json
{
  "id": 1,
  "createTime": "2025-10-28T10:39:00.812+08:00",
  "updateTime": "2025-10-28T10:39:00.812+08:00",
  "name": "espresso",
  "price": 100.00
}
```

**Create coffee:**
```bash
curl -X POST http://localhost:8080/coffee/ \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Americano",
    "price": 125.00
  }'
```

### Order Management

**Create order:**
```bash
curl -X POST http://localhost:8080/order/ \
  -H "Content-Type: application/json" \
  -d '{
    "customer": "John Doe",
    "items": ["espresso", "latte"]
  }'
```

**Get order:**
```bash
curl http://localhost:8080/order/1
```

## Interceptor Use Cases

### 1. Performance Monitoring

**Current Implementation:**
- Track request processing time
- Identify slow endpoints
- Log performance metrics

**Extension:**
```java
@Override
public void afterCompletion(...) {
    long totalTime = sw.getTotalTimeMillis();
    
    // Alert if request takes too long
    if (totalTime > 1000) {
        log.warn("Slow request detected: {} took {}ms", 
                request.getRequestURI(), totalTime);
    }
}
```

### 2. Authentication & Authorization

```java
@Override
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, 
                        Object handler) {
    String token = request.getHeader("Authorization");
    
    if (token == null || !isValidToken(token)) {
        response.setStatus(HttpStatus.UNAUTHORIZED.value());
        return false;  // Terminate request
    }
    
    return true;  // Continue processing
}
```

### 3. Request Logging

```java
@Override
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, 
                        Object handler) {
    log.info("Incoming request: {} {} from {}", 
            request.getMethod(),
            request.getRequestURI(),
            request.getRemoteAddr());
    return true;
}
```

### 4. Response Modification

```java
@Override
public void postHandle(HttpServletRequest request, HttpServletResponse response, 
                      Object handler, ModelAndView modelAndView) {
    // Add custom headers
    response.addHeader("X-Custom-Header", "Custom Value");
    
    // Modify model data
    if (modelAndView != null) {
        modelAndView.addObject("timestamp", System.currentTimeMillis());
    }
}
```

### 5. Cache Control

```java
@Override
public void afterCompletion(HttpServletRequest request, HttpServletResponse response, 
                           Object handler, Exception ex) {
    // Invalidate cache after POST/PUT/DELETE
    if ("POST".equals(request.getMethod()) || 
        "PUT".equals(request.getMethod()) || 
        "DELETE".equals(request.getMethod())) {
        cacheService.invalidate(request.getRequestURI());
    }
}
```

## HandlerInterceptor vs Filter

| Feature | HandlerInterceptor | Servlet Filter |
|---------|-------------------|----------------|
| Framework | Spring MVC | Java Servlet API |
| Handler Access | ✅ Yes (HandlerMethod) | ❌ No |
| Execution Scope | After DispatcherServlet | Before DispatcherServlet |
| Spring Context | ✅ Full access | ⚠️ Limited |
| Use Case | MVC-specific logic | General request/response processing |
| Exception Handling | ✅ Can access controller exceptions | ❌ Cannot |
| Performance | Slightly slower | Faster |

**When to Use:**
- **HandlerInterceptor**: MVC-specific concerns (authentication, authorization, performance monitoring)
- **Filter**: General concerns (compression, encryption, CORS, encoding)

## Best Practices

### 1. ThreadLocal Cleanup

**⚠️ Always clean up ThreadLocal:**
```java
@Override
public void afterCompletion(...) {
    try {
        // Your logic
    } finally {
        stopWatch.remove();  // Critical: prevent memory leak
    }
}
```

### 2. Interceptor Order

```java
@Override
public void addInterceptors(InterceptorRegistry registry) {
    registry.addInterceptor(new AuthenticationInterceptor())
            .order(1);  // Execute first
    
    registry.addInterceptor(new PerformanceInteceptor())
            .order(2);  // Execute second
}
```

### 3. Path Pattern Matching

```java
registry.addInterceptor(interceptor)
        .addPathPatterns("/api/**")        // All under /api
        .excludePathPatterns("/api/public/**")  // Except /api/public
        .excludePathPatterns("/api/health");    // Except /api/health
```

### 4. Async Request Handling

**⚠️ Important: Async requests don't execute `postHandle` and `afterCompletion`**

```java
@GetMapping("/async")
public CompletableFuture<String> asyncEndpoint() {
    // postHandle and afterCompletion won't be called
    return CompletableFuture.completedFuture("result");
}
```

### 5. Exception Handling

```java
@Override
public void afterCompletion(..., Exception ex) {
    if (ex != null) {
        log.error("Request failed: {} - {}", 
                request.getRequestURI(), 
                ex.getMessage());
        
        // Send to monitoring system
        monitoringService.recordError(ex);
    }
}
```

## Testing

### Unit Test Example

```java
@SpringBootTest
class WaiterServiceApplicationTests {

    @Test
    void contextLoads() {
        // Verify application context loads successfully
    }
}
```

### Test Performance Interceptor

```bash
# Make a request and observe logs
curl http://localhost:8080/coffee/1

# Expected log output:
# /coffee/1;...CoffeeController.getById;200;-;86ms;86ms;0ms
```

## Monitoring

### Performance Metrics Analysis

**Parse log files to analyze performance:**
```bash
# Extract slow requests (>100ms)
grep "PerformanceInteceptor" logs/application.log | \
  awk -F';' '$5 > 100' | \
  sort -t';' -k5 -nr

# Top 10 slowest endpoints
grep "PerformanceInteceptor" logs/application.log | \
  awk -F';' '{print $1, $5}' | \
  sort -k2 -nr | \
  head -10
```

### Enable Debug Logging

```properties
# application.properties
logging.level.tw.fengqing.spring.springbucks.waiter.controller=DEBUG
```

## Common Issues

### Issue 1: ThreadLocal Memory Leak

**Problem:**
```
java.lang.OutOfMemoryError: unable to create new native thread
```

**Solution:**
```java
// Always remove ThreadLocal in finally block
@Override
public void afterCompletion(...) {
    try {
        // Your logic
    } finally {
        stopWatch.remove();  // Critical!
    }
}
```

### Issue 2: Interceptor Not Executing

**Problem:**
Interceptor not being called

**Solutions:**
```java
// 1. Check path patterns
registry.addInterceptor(interceptor)
        .addPathPatterns("/**");  // Match all paths

// 2. Verify interceptor is registered
@Override
public void addInterceptors(InterceptorRegistry registry) {
    // Make sure this method is called
}

// 3. Check if @Configuration is present
@Configuration  // Don't forget this!
public class WebMvcConfig implements WebMvcConfigurer {
    // ...
}
```

### Issue 3: Async Requests Not Logged

**Problem:**
Performance logs missing for async endpoints

**Solution:**
Use `AsyncHandlerInterceptor` instead:
```java
public class AsyncPerformanceInterceptor implements AsyncHandlerInterceptor {
    
    @Override
    public void afterConcurrentHandlingStarted(...) {
        // Handle async requests
    }
}
```

## Dependencies

```xml
<properties>
    <java.version>21</java.version>
    <joda-money.version>2.0.2</joda-money.version>
</properties>

<dependencies>
    <!-- Spring Boot Web (includes Spring MVC) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    
    <!-- Spring Data JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    
    <!-- Spring Cache -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-cache</artifactId>
    </dependency>
    
    <!-- H2 Database -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
    
    <!-- Joda Money -->
    <dependency>
        <groupId>org.joda</groupId>
        <artifactId>joda-money</artifactId>
        <version>${joda-money.version}</version>
    </dependency>
    
    <!-- Jackson for Hibernate -->
    <dependency>
        <groupId>com.fasterxml.jackson.datatype</groupId>
        <artifactId>jackson-datatype-hibernate6</artifactId>
    </dependency>
    
    <!-- Lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```

## Best Practices Demonstrated

1. **HandlerInterceptor**: Performance monitoring with custom interceptor
2. **ThreadLocal Management**: Proper ThreadLocal cleanup to prevent memory leaks
3. **Request Lifecycle**: Complete request tracking (pre/post/after)
4. **Performance Logging**: Structured logging for performance analysis
5. **MVC Configuration**: Proper interceptor registration and path patterns
6. **Layered Architecture**: Clear separation of concerns (Controller/Service/Repository)
7. **Data Validation**: Bean Validation for data integrity

## References

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Spring MVC Interceptors](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-config/interceptors.html)
- [HandlerInterceptor API](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerInterceptor.html)
- [Spring Data JPA](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
- [Joda Money Documentation](https://www.joda.org/joda-money/)

## License

MIT License - see [LICENSE](LICENSE) file for details.

## About Us

我們主要專注在敏捷專案管理、物聯網（IoT）應用開發和領域驅動設計（DDD）。喜歡把先進技術和實務經驗結合，打造好用又靈活的軟體解決方案。近來也積極結合 AI 技術，推動自動化工作流，讓開發與運維更有效率、更智慧。持續學習與分享，希望能一起推動軟體開發的創新和進步。

## Contact

**風清雲談** - 專注於敏捷專案管理、物聯網（IoT）應用開發和領域驅動設計（DDD）。

- 🌐 官方網站：[風清雲談部落格](https://blog.fengqing.tw/)
- 📘 Facebook：[風清雲談粉絲頁](https://www.facebook.com/profile.php?id=61576838896062)
- 💼 LinkedIn：[Chu Kuo-Lung](https://www.linkedin.com/in/chu-kuo-lung)
- 📺 YouTube：[雲談風清頻道](https://www.youtube.com/channel/UCXDqLTdCMiCJ1j8xGRfwEig)
- 📧 Email：[fengqing.tw@gmail.com](mailto:fengqing.tw@gmail.com)

---

**⭐ If this project helps you, please give it a star!**
