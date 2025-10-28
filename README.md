# SpringBucks Coffee Shop Service System ⚡

[![Java](https://img.shields.io/badge/Java-21-orange.svg)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.5-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Spring Data JPA](https://img.shields.io/badge/Spring%20Data%20JPA-3.4-blue.svg)](https://spring.io/projects/spring-data-jpa)
[![H2 Database](https://img.shields.io/badge/H2%20Database-2.2.224-yellow.svg)](https://www.h2database.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Project Overview

SpringBucks is a coffee shop management system built on Spring Boot 3.x, providing coffee product management, order processing, and RESTful API services. This project demonstrates modern Spring Boot microservice architecture best practices, including complete data persistence, caching mechanisms, and performance monitoring.

### 🎯 Core Features
- **Coffee Product Management** - Create, query, and update coffee product information
- **Order Processing System** - Create orders and track order status
- **RESTful API** - Comprehensive HTTP API interface
- **Performance Monitoring** - Built-in request performance interceptor
- **Data Persistence** - Database operations using JPA/Hibernate
- **Caching Mechanism** - Integrated Spring Cache to enhance system performance

### 💡 Why Choose This Project?
- **Modern Technology Stack** - Built with Spring Boot 3.x + Java 21 + Jakarta EE
- **Complete Feature Showcase** - Covers core functionalities including Web, JPA, Cache, Validation, etc.
- **Best Practices** - Follows Spring Boot official recommended project structure and configuration
- **Easy to Learn** - Clear code structure with comprehensive comments, suitable for learning Spring Boot development

### 🚀 Project Highlights

- **Layered Architecture Design** - Clear separation of Controller, Service, and Repository layers
- **Data Validation Mechanism** - Uses Bean Validation to ensure data correctness
- **Custom Type Conversion** - Integrates Joda Money for currency calculations
- **Performance Optimization** - Built-in caching and performance monitoring mechanisms
- **Request Interceptor** - Implements HandlerInterceptor for performance monitoring and logging
- **Cross-platform Support** - Uses H2 in-memory database, no additional installation required

## Technology Stack

### Core Frameworks
- **Spring Boot 3.4.5** - Modern Java application framework
- **Spring Data JPA** - Data persistence and ORM framework
- **Spring Web MVC** - Web application development framework
- **Spring Cache** - Cache abstraction layer and implementation

### Database & Tools
- **H2 Database** - Lightweight in-memory database
- **Hibernate 6** - JPA implementation, supports Jakarta EE
- **Joda Money** - Currency calculation and processing library

### Development Tools & Utilities
- **Lombok** - Reduces boilerplate code, improves development efficiency
- **Jackson** - JSON/XML serialization and deserialization
- **Bean Validation** - Data validation framework
- **Apache Commons Lang3** - Common utility library

## Project Structure

```
springbucks2/
├── src/
│   ├── main/
│   │   ├── java/tw/fengqing/spring/springbucks/waiter/
│   │   │   ├── WaiterServiceApplication.java      # Main application class
│   │   │   ├── controller/          # Controller layer - HTTP request handling
│   │   │   │   ├── CoffeeController.java
│   │   │   │   ├── CoffeeOrderController.java
│   │   │   │   ├── PerformanceInteceptor.java     # Performance monitoring interceptor
│   │   │   │   └── request/         # Request objects
│   │   │   ├── service/             # Service layer - Business logic
│   │   │   │   ├── CoffeeService.java
│   │   │   │   └── CoffeeOrderService.java
│   │   │   ├── repository/          # Data access layer - Database operations
│   │   │   │   ├── CoffeeRepository.java
│   │   │   │   └── CoffeeOrderRepository.java
│   │   │   ├── model/               # Entity models - Data structure definition
│   │   │   │   ├── Coffee.java
│   │   │   │   ├── CoffeeOrder.java
│   │   │   │   ├── BaseEntity.java
│   │   │   │   ├── OrderState.java
│   │   │   │   └── MoneyConverter.java
│   │   │   └── support/             # Support classes - Utilities and configuration
│   │   │       ├── MoneySerializer.java
│   │   │       ├── MoneyDeserializer.java
│   │   │       └── MoneyFormatter.java
│   │   └── resources/
│   │       ├── application.properties  # Application configuration
│   │       ├── schema.sql              # Database schema
│   │       ├── data.sql                # Initial data
│   │       └── coffee.txt              # Coffee data file
│   └── test/                        # Test code
├── pom.xml                          # Maven project configuration
└── README.md                        # Project documentation
```

## Quick Start

### Prerequisites
- **Java 21** - Ensure JDK 21 or newer is installed
- **Maven 3.6+** - Project build tool
- **IDE Recommendation** - IntelliJ IDEA, Eclipse, or VS Code

### Installation & Execution

**1. Clone this repository:**
```bash
git clone <repository-url>
cd springbucks2
```

**2. Compile the project:**
```bash
mvn clean compile
```

**3. Run the application:**
```bash
mvn spring-boot:run
```

**4. Verify service startup:**
```bash
curl http://localhost:8080/coffee/1
```

### Package & Deploy
```bash
# Build executable JAR file
mvn clean package

# Run JAR file
java -jar target/waiter-service-0.0.1-SNAPSHOT.jar
```

## API Usage Guide

### Coffee Product Management

#### Query All Coffees
```bash
GET /coffee/
```

#### Query Specific Coffee
```bash
GET /coffee/{id}
```

**Example Response:**
```json
{
  "id": 1,
  "createTime": "2025-10-28T10:39:00.812731+08:00",
  "updateTime": "2025-10-28T10:39:00.812731+08:00",
  "name": "espresso",
  "price": 100.00
}
```

#### Create New Coffee
```bash
POST /coffee/
Content-Type: application/json

{
  "name": "Americano",
  "price": 125.00
}
```

### Order Management

#### Create New Order
```bash
POST /order/
Content-Type: application/json

{
  "customer": "John Doe",
  "items": ["Espresso", "Latte"]
}
```

#### Query Order
```bash
GET /order/{id}
```

## Advanced Configuration

### Environment Variables
```properties
# Database configuration (default uses H2 in-memory database)
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver

# JPA configuration
spring.jpa.hibernate.ddl-auto=none
spring.jpa.show-sql=true
spring.jpa.format-sql=true

# Application configuration
server.port=8080
spring.jackson.time-zone=Asia/Taipei
```

### Configuration File Description

#### application.properties Main Settings
```properties
# JPA configuration - Controls database structure generation
spring.jpa.hibernate.ddl-auto=none

# SQL logging - Display SQL statements during development
spring.jpa.properties.hibernate.show_sql=true
spring.jpa.properties.hibernate.format_sql=true

# Error message display (development environment)
server.error.include-message=always
server.error.include-binding-errors=always
```

### Cache Configuration
The project has Spring Cache enabled, using in-memory cache by default. Can be customized as follows:
```java
@Cacheable("coffee")
public Coffee getCoffeeById(Long id) {
    // Cache logic
}
```

## Spring MVC Interceptor Mechanism

The project implements a custom performance monitoring interceptor for tracking request processing time and performance analysis.

### Interceptor Lifecycle
```java
public interface HandlerInterceptor {
    // Pre-processing before method execution
    boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler);
    
    // Post-processing after method execution, before view rendering
    void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView);
    
    // Processing after entire request completion (including view rendering)
    void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex);
}
```

### Interceptor Configuration
```java
@SpringBootApplication
@EnableJpaRepositories
@EnableCaching
public class WaiterServiceApplication implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new PerformanceInteceptor())
                .addPathPatterns("/coffee/**")
                .addPathPatterns("/order/**");
    }
}
```

### Performance Monitoring Interceptor Implementation
```java
@Slf4j
public class PerformanceInteceptor implements HandlerInterceptor {
    private ThreadLocal<StopWatch> stopWatch = new ThreadLocal<>();

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        // Record request start time
        StopWatch sw = new StopWatch();
        stopWatch.set(sw);
        sw.start();
        return true; // Return true to continue, false to terminate request
    }
    
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) {
        stopWatch.get().stop();
        stopWatch.get().start();
    }
    
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        // Calculate total processing time and log
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
        
        stopWatch.remove();
    }
}
```

**Example Log Output:**
```log
2025-10-28T10:40:36.538+08:00  INFO 72109 --- [nio-8080-exec-2] t.f.s.s.w.c.PerformanceInteceptor        : /coffee/1;tw.fengqing.spring.springbucks.waiter.controller.CoffeeController.getById;200;-;86ms;86ms;0ms
```

**Log Field Explanation:**
- `/coffee/1`: Request URL
- `CoffeeController.getById`: Executed method
- `200`: HTTP status code
- `-`: No exception (displays exception class name if any)
- `86ms`: Total time
- `86ms`: Controller execution time
- `0ms`: View rendering time

### Interceptor Use Cases
- **Permission Verification** - Check user permissions in `preHandle`
- **Performance Monitoring** - Record request processing time and performance metrics
- **Logging** - Record request details and processing results
- **Cache Handling** - Update cache data in `postHandle`
- **Exception Handling** - Unified exception handling in `afterCompletion`

### Important Notes
- **Async Requests** - Async processing won't execute `postHandle` and `afterCompletion`
- **Interceptor Order** - Set execution order via `order()` method
- **Path Matching** - Supports Ant-style path pattern matching
- **Performance Impact** - Interceptors add minor performance overhead, use wisely
- **ThreadLocal Cleanup** - Must call `remove()` to prevent memory leaks

## Development Guide

### Steps to Add New Features
1. **Create Entity Model** - Define data structure in `model` package
2. **Create Repository** - Define data access interface in `repository` package
3. **Create Service** - Implement business logic in `service` package
4. **Create Controller** - Define API endpoints in `controller` package
5. **Write Tests** - Ensure functionality correctness

### Code Conventions
- **Naming Convention** - Use camelCase, capitalize class names
- **Documentation** - Add clear comments for important logic
- **Layered Architecture** - Strictly follow Controller → Service → Repository call sequence
- **Exception Handling** - Use unified exception handling mechanism

## References

- [Spring Boot Official Documentation](https://spring.io/projects/spring-boot)
- [Spring Data JPA Reference Guide](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
- [Hibernate Official Documentation](https://hibernate.org/orm/documentation/)
- [Joda Money Documentation](https://www.joda.org/joda-money/)
- [Spring MVC Interceptors](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-config/interceptors.html)

## Important Notes & Best Practices

### ⚠️ Important Reminders

| Item | Description | Recommended Approach |
|------|-------------|---------------------|
| Database Connection | Production database configuration | Use external database (e.g., PostgreSQL, MySQL) |
| Cache Strategy | Cache invalidation and updates | Implement cache update mechanism |
| Performance Monitoring | Request performance tracking | Regularly review performance logs |
| Data Validation | Input data validation | Use Bean Validation to ensure data correctness |
| ThreadLocal Cleanup | Memory leak prevention | Always call `remove()` in `afterCompletion()` |

### 🔒 Best Practice Guidelines

- **Layered Architecture** - Strictly follow MVC architecture, maintain separation of concerns
- **Data Validation** - Perform input validation at Controller layer to ensure data integrity
- **Exception Handling** - Implement unified exception handling mechanism with user-friendly error messages
- **Performance Optimization** - Use caching mechanisms wisely, avoid redundant database queries
- **Interceptor Design** - Use HandlerInterceptor for cross-cutting concerns like permission verification and performance monitoring
- **Code Quality** - Regularly refactor code to maintain readability and maintainability

### 🛠️ Development Recommendations

- **IDE Setup** - Recommended to use IntelliJ IDEA or Eclipse with Lombok plugin installed
- **Debugging Tips** - Make good use of Spring Boot DevTools and hot reload features
- **Testing Strategy** - Write unit tests and integration tests to ensure code quality
- **Version Control** - Use Git for version control and follow Git Flow workflow

## License

This project is licensed under the MIT License. See LICENSE file for details.

## About Us

We specialize in Agile Project Management, IoT application development, and Domain-Driven Design (DDD). We enjoy combining advanced technologies with practical experience to create user-friendly and flexible software solutions.

## Contact Us

- **Facebook Page**: [風清雲談 | Facebook](https://www.facebook.com/profile.php?id=61576838896062)
- **LinkedIn**: [linkedin.com/in/chu-kuo-lung](https://www.linkedin.com/in/chu-kuo-lung)
- **YouTube Channel**: [雲談風清 - YouTube](https://www.youtube.com/channel/UCXDqLTdCMiCJ1j8xGRfwEig)
- **Blog**: [風清雲談](https://blog.fengqing.tw/)
- **Email**: [fengqing.tw@gmail.com](mailto:fengqing.tw@gmail.com)

---

**📅 Last Updated: October 28, 2025**  
**👨‍💻 Maintained by: FengQing Team**
