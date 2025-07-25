# SpringBucks 咖啡廳服務系統 ⚡

[![Java](https://img.shields.io/badge/Java-21-orange.svg)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20-3.40.5brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Spring Data JPA](https://img.shields.io/badge/Spring%20a%20JPA-3.45lue.svg)](https://spring.io/projects/spring-data-jpa)
[![H2 Database](https://img.shields.io/badge/H2%20Database-2.20.224yellow.svg)](https://www.h2database.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 專案介紹

SpringBucks 是一個基於 Spring Boot3.x 的咖啡廳管理系統，提供咖啡商品管理、訂單處理、以及 RESTful API 服務。此專案展示了現代 Spring Boot 微服務架構的最佳實踐，包含完整的資料持久化、快取機制、效能監控等功能。

### 🎯 核心功能
- **咖啡商品管理** - 新增、查詢、更新咖啡商品資訊
- **訂單處理系統** - 建立訂單、追蹤訂單狀態
- **RESTful API** - 提供完整的 HTTP API 介面
- **效能監控** - 內建請求效能攔截器
- **資料持久化** - 使用 JPA/Hibernate 進行資料庫操作
- **快取機制** - 整合 Spring Cache 提升系統效能

### 💡 為什麼選擇此專案？
- **現代化技術棧** - 採用 Spring Boot 3.x + Java21 + Jakarta EE
- **完整的功能展示** - 涵蓋 Web、JPA、Cache、Validation 等核心功能
- **最佳實踐** - 遵循 Spring Boot 官方建議的專案結構與配置
- **易於學習** - 程式碼結構清晰，註解完整，適合學習 Spring Boot 開發

### 🚀 專案特色

- **分層架構設計** - Controller、Service、Repository 清晰分離
- **資料驗證機制** - 使用 Bean Validation 確保資料正確性
- **自訂型別轉換** - 整合 Joda Money 處理貨幣計算
- **效能優化** - 內建快取與效能監控機制
- **請求攔截器** - 實作 HandlerInterceptor 進行效能監控與日誌記錄
- **跨平台支援** - 使用 H2 記憶體資料庫，無需額外安裝

## 技術棧

### 核心框架
- **Spring Boot 30.40.5** - 現代化 Java 應用程式框架
- **Spring Data JPA** - 資料持久化與 ORM 框架
- **Spring Web MVC** - Web 應用程式開發框架
- **Spring Cache** - 快取抽象層與實作

### 資料庫與工具
- **H2Database** - 輕量級記憶體資料庫
- **Hibernate6 JPA 實作，支援 Jakarta EE
- **Joda Money** - 貨幣計算與處理函式庫

### 開發工具與輔助
- **Lombok** - 減少樣板程式碼，提升開發效率
- **Jackson** - JSON/XML 序列化與反序列化
- **Bean Validation** - 資料驗證框架
- **Apache Commons Lang3** - 通用工具函式庫

## 專案結構

```
springbucks/
├── src/
│   ├── main/
│   │   ├── java/tw/fengqing/spring/springbucks/waiter/
│   │   │   ├── controller/          # 控制器層 - 處理 HTTP 請求
│   │   │   │   ├── CoffeeController.java
│   │   │   │   ├── CoffeeOrderController.java
│   │   │   │   └── request/         # 請求物件
│   │   │   ├── service/             # 服務層 - 業務邏輯處理
│   │   │   │   ├── CoffeeService.java
│   │   │   │   └── CoffeeOrderService.java
│   │   │   ├── repository/          # 資料存取層 - 資料庫操作
│   │   │   │   ├── CoffeeRepository.java
│   │   │   │   └── CoffeeOrderRepository.java
│   │   │   ├── model/               # 實體模型 - 資料結構定義
│   │   │   │   ├── Coffee.java
│   │   │   │   ├── CoffeeOrder.java
│   │   │   │   ├── BaseEntity.java
│   │   │   │   ├── OrderState.java
│   │   │   │   └── MoneyConverter.java
│   │   │   └── support/             # 支援類別 - 工具與配置
│   │   │       └── MoneySerializer.java
│   │   └── resources/
│   │       ├── application.properties  # 應用程式配置
│   │       ├── schema.sql              # 資料庫結構
│   │       ├── data.sql                # 初始資料
│   │       └── coffee.txt              # 咖啡資料檔案
│   └── test/                        # 測試程式碼
├── pom.xml                          # Maven 專案配置
└── README.md                        # 專案說明文件
```

## 快速開始

### 前置需求
- **Java21** - 確保已安裝 JDK 21 或更新版本
- **Maven36* - 專案建置工具
- **IDE 建議** - IntelliJ IDEA、Eclipse 或 VS Code

### 安裝與執行
1**克隆此倉庫：**
```bash
git clone <repository-url>
cd springbucks
```2 **編譯專案：**
```bash
mvn clean compile
```

3*執行應用程式：**
```bash
mvn spring-boot:run
```

4*驗證服務啟動：**
```bash
curl http://localhost:880/coffee/1
```

### 打包部署
```bash
# 建立可執行 JAR 檔案
mvn clean package

# 執行 JAR 檔案
java -jar target/waiter-service-00SNAPSHOT.jar
```

## API 使用說明

### 咖啡商品管理

#### 查詢所有咖啡
```bash
GET /coffee
```

#### 查詢特定咖啡
```bash
GET /coffee/{id}
```

#### 新增咖啡
```bash
POST /coffee
Content-Type: application/json
[object Object] name": 美式咖啡",
  "price: 50
}
```

### 訂單管理

#### 建立新訂單
```bash
POST /order
Content-Type: application/json

[object Object]
  customer": "張三",
 items": ["1", 2]
}
```

#### 查詢訂單
```bash
GET /order/{id}
```

## 進階說明

### 環境變數
```properties
# 資料庫配置（預設使用 H2 記憶體資料庫）
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver

# JPA 配置
spring.jpa.hibernate.ddl-auto=none
spring.jpa.show-sql=true
spring.jpa.format-sql=true

# 應用程式配置
server.port=880pring.jackson.time-zone=Asia/Taipei
```

### 設定檔說明

#### application.properties 主要設定
```properties
# JPA 配置 - 控制資料庫結構生成
spring.jpa.hibernate.ddl-auto=none

# SQL 日誌 - 開發時顯示 SQL 語句
spring.jpa.properties.hibernate.show_sql=true
spring.jpa.properties.hibernate.format_sql=true
```

### 快取配置
專案已啟用 Spring Cache，預設使用記憶體快取。可透過以下方式自訂：
```java
@Cacheable("coffee")
public Coffee getCoffeeById(Long id) {
    // 快取邏輯
}
```

### Spring MVC 攔截器機制

專案實作了自訂的效能監控攔截器，用於追蹤請求處理時間與效能分析。

#### 攔截器生命週期
```java
public interface HandlerInterceptor [object Object]
    // 方法執行前的預處理
    boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler);
    
    // 方法執行後，視圖渲染前的處理
    void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView);
    
    // 整個請求完成後的處理（包含視圖渲染）
    void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex);
}
```

#### 攔截器配置
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) [object Object]      registry.addInterceptor(new PerformanceInterceptor())
                .addPathPatterns("/coffee/**",/order/**");
    }
}
```

#### 效能監控攔截器實作
```java
@Component
public class PerformanceInterceptor implements HandlerInterceptor {
    
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)[object Object]
        // 記錄請求開始時間
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();
        request.setAttribute("stopWatch", stopWatch);
        return true; // 返回 true 繼續處理，false 則終止請求
    }
    
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) [object Object]        // 計算總處理時間並記錄日誌
        StopWatch stopWatch = (StopWatch) request.getAttribute("stopWatch");
        stopWatch.stop();
        
        log.info(請求處理摘要 - URL: {}, 方法: {}, 狀態: [object Object]}, 總耗時: {}ms, 方法耗時: {}ms,           request.getRequestURI(),
                handler.getClass().getSimpleName(),
                response.getStatus(),
                stopWatch.getTotalTimeMillis(),
                stopWatch.getLastTaskTimeMillis());
    }
}
```

#### 攔截器使用場景
- **權限驗證** - 在 `preHandle` 中檢查用戶權限
- **效能監控** - 記錄請求處理時間與效能指標
- **日誌記錄** - 記錄請求詳情與處理結果
- **快取處理** - 在 `postHandle` 中更新快取資料
- **異常處理** - 在 `afterCompletion` 中統一處理異常

#### 注意事項
- **非同步請求** - 非同步處理不會執行 `postHandle` 和 `afterCompletion`
- **攔截器順序** - 可透過 `order()` 方法設定攔截器執行順序
- **路徑匹配** - 支援 Ant 風格的路徑模式匹配
- **效能影響** - 攔截器會增加少量效能開銷，建議合理使用

## 開發指南

### 新增功能步驟1. **建立實體模型** - 在 `model` 包下定義資料結構
2 **建立 Repository** - 在 `repository` 包下定義資料存取介面3. **建立 Service** - 在 `service` 包下實作業務邏輯
4 **建立 Controller** - 在 `controller` 包下定義 API 端點
5 **撰寫測試** - 確保功能正確性

### 程式碼規範
- **命名規範** - 使用駝峰命名法，類別首字母大寫
- **註解規範** - 重要邏輯需加上清楚註解
- **分層架構** - 嚴格遵守 Controller → Service → Repository 的呼叫順序
- **異常處理** - 使用統一的異常處理機制

## 參考資源

-Spring Boot 官方文件](https://spring.io/projects/spring-boot)
-Spring Data JPA 參考指南](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
- [Hibernate 官方文件](https://hibernate.org/orm/documentation/)
- [Joda Money 文件](https://www.joda.org/joda-money/)

## 注意事項與最佳實踐

### ⚠️ 重要提醒

| 項目 | 說明 | 建議做法 |
|------|------|----------|
| 資料庫連線 | 生產環境資料庫配置 | 使用外部資料庫（如 PostgreSQL、MySQL） |
| 快取策略 | 快取失效與更新 | 實作快取更新機制 |
| 效能監控 | 請求效能追蹤 | 定期檢查效能日誌 |
| 資料驗證 | 輸入資料驗證 | 使用 Bean Validation 確保資料正確性 |

### 🔒 最佳實踐指南

- **分層架構** - 嚴格遵守 MVC 架構，保持各層職責分離
- **資料驗證** - 在 Controller 層進行輸入驗證，確保資料完整性
- **異常處理** - 實作統一的異常處理機制，提供友善的錯誤訊息
- **效能優化** - 合理使用快取機制，避免重複的資料庫查詢
- **攔截器設計** - 使用 HandlerInterceptor 進行橫切關注點處理，如權限驗證、效能監控
- **程式碼品質** - 定期重構程式碼，保持程式碼的可讀性與可維護性

### 🛠️ 開發建議

- **IDE 設定** - 建議使用 IntelliJ IDEA 或 Eclipse，並安裝 Lombok 外掛
- **除錯技巧** - 善用 Spring Boot 的開發工具與熱重載功能
- **測試策略** - 撰寫單元測試與整合測試，確保程式碼品質
- **版本控制** - 使用 Git 進行版本控制，並遵循 Git Flow 工作流程

## 授權說明

本專案採用 MIT 授權條款，詳見 LICENSE 檔案。

## 關於我們

我們主要專注在敏捷專案管理、物聯網（IoT）應用開發和領域驅動設計（DDD）。喜歡把先進技術和實務經驗結合，打造好用又靈活的軟體解決方案。

## 聯繫我們

- **FB 粉絲頁**：[風清雲談 | Facebook](https://www.facebook.com/profile.php?id=61576838896062)
- **LinkedIn**：[linkedin.com/in/chu-kuo-lung](https://www.linkedin.com/in/chu-kuo-lung)
- **YouTube 頻道**：[雲談風清 - YouTube](https://www.youtube.com/channel/UCXDqLTdCMiCJ1j8xGRfwEig)
- **風清雲談 部落格**：[風清雲談](https://blog.fengqing.tw/)
- **電子郵件**：[fengqing.tw@gmail.com](mailto:fengqing.tw@gmail.com)

---

**📅 最後更新：2025年7月17日  
**👨‍💻 維護者：風清雲談團隊** 