# **What is Spring Boot?**  

**Spring Boot** is an open-source, Java-based framework designed to simplify the development of **standalone, production-ready Spring applications** with minimal configuration. It is built on top of the **Spring Framework** and follows the principle of **"Convention over Configuration"**, reducing boilerplate code and allowing developers to focus on business logic rather than setup and infrastructure.  

---

## **Key Features of Spring Boot**  

### **1. Auto-Configuration**  
- Automatically configures Spring and third-party libraries based on **dependencies** in the classpath.  
- Example: If `spring-boot-starter-data-jpa` is added, Spring Boot auto-configures **Hibernate, DataSource, and JPA**.  

### **2. Standalone Applications**  
- **No need for external servers** (like Tomcat).  
- Comes with **embedded servers** (Tomcat, Jetty, or Undertow).  
- Runs as a **self-contained JAR** (Java Archive) with `java -jar`.  

### **3. Starter Dependencies (Starter POMs)**  
- Simplifies dependency management (e.g., `spring-boot-starter-web` includes Spring MVC, Tomcat, and JSON support).  
- Avoids version conflicts.  

### **4. No XML Configuration**  
- Uses **Java-based annotations** (`@SpringBootApplication`) instead of XML.  

### **5. Production-Ready Features**  
- **Spring Boot Actuator** → Provides monitoring (health checks, metrics, logs).  
- **Externalized Configuration** → Supports `.properties` and `.yml` files.  
- **Logging** → Built-in support for **Logback, SLF4J**.  

---

## **Why Use Spring Boot?**  
| **Before Spring Boot** | **With Spring Boot** |  
|------------------------|----------------------|  
| Manual XML/Java Config (`applicationContext.xml`) | **Auto-Configuration** (Zero config) |  
| Manually add dependencies (risk of conflicts) | **Starter POMs** (Pre-defined dependencies) |  
| Deploy WAR on external servers (Tomcat) | **Embedded Server** (Self-contained JAR) |  
| Slow setup, more boilerplate code | **Faster development** (Focus on business logic) |  

---

## **Example: Simple Spring Boot Application**  
```java
@SpringBootApplication  // Auto-config + Component Scan
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args); // Starts embedded server
    }
}

@RestController
class HelloController {
    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, Spring Boot!";
    }
}
```
- Just **one class** with `@SpringBootApplication` → Runs a full REST API!  
- No `web.xml`, no server setup.  

---

# **Spring Boot vs. Spring Framework: Key Differences**

## **1. Overview**
| Feature          | Spring Framework | Spring Boot |
|-----------------|----------------|-------------|
| **Purpose** | Core framework for dependency injection, AOP, MVC | Opinionated extension of Spring for rapid app development |
| **Configuration** | Manual (XML/Java-based) | Auto-configuration |
| **Embedded Server** | No (requires external server) | Yes (Tomcat/Jetty/Undertow) |
| **Dependency Mgmt** | Manual | Starter POMs (pre-configured) |
| **Bootstrapping** | Complex setup | `@SpringBootApplication` (single annotation) |
| **Best For** | Flexible, fine-grained control | Rapid development, microservices |

## **2. Key Differences Explained**

### **A. Configuration Approach**
- **Spring Framework**:  
  - Requires explicit configuration (XML or `@Configuration` classes)  
  - Example:  
    ```xml
    <!-- Manual bean definition in XML -->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource"/>
    ```
  
- **Spring Boot**:  
  - **Auto-configures** beans based on classpath dependencies  
  - Example: Just add `spring-boot-starter-data-jpa` → Auto-configures Hibernate!  

### **B. Project Setup**
- **Spring Framework**:  
  - Manual dependency management (risk of version conflicts)  
  - Requires separate server deployment (WAR files)  

- **Spring Boot**:  
  - **Starter dependencies** (e.g., `spring-boot-starter-web` bundles Spring MVC + Tomcat)  
  - Embedded server → Runs as **self-contained JAR**  

### **C. Development Speed**
- **Spring Framework**:  
  ```java
  @Configuration
  @EnableWebMvc
  public class AppConfig implements WebMvcConfigurer { 
      // Manual MVC configuration
  }
  ```
  
- **Spring Boot**:  
  ```java
  @SpringBootApplication // Single annotation replaces all boilerplate
  public class MyApp { 
      public static void main(String[] args) {
          SpringApplication.run(MyApp.class, args); 
      }
  }
  ```

### **D. Production Features**
| Feature          | Spring Framework | Spring Boot |
|-----------------|----------------|-------------|
| **Actuator** | Not available | Built-in (health checks, metrics) |
| **Profiles** | Manual setup | Native support (`application-{profile}.yml`) |
| **Logging** | Manual configuration | Auto-configured (Logback/SLF4J) |

## **3. When to Use Which?**
### **Choose Spring Framework When:**
- You need **full control** over configurations  
- Working with **legacy systems** requiring XML  
- Building **highly customized** architectures  

### **Choose Spring Boot When:**
- Rapid prototyping or microservices development  
- Avoiding boilerplate configuration  
- Need **embedded servers** or **Actuator** for monitoring  

## **4. Code Comparison**
### **Spring Framework (Web App)**
```java
public class MyWebInitializer implements WebApplicationInitializer {
    @Override
    public void onStartup(ServletContext container) {
        AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
        context.register(AppConfig.class);
        container.addListener(new ContextLoaderListener(context));
        // Manual DispatcherServlet setup...
    }
}
```

### **Spring Boot (Web App)**
```java
@SpringBootApplication // Auto-configures EVERYTHING
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args); 
    }
}
```

# **Spring Boot Architecture: Deep Dive**

## **1. Overview of Spring Boot Architecture**
Spring Boot follows a **layered architecture** built on top of the Spring Framework. Its key components work together to provide:
- **Auto-configuration** (Smart defaults)
- **Embedded server** (No external deployment)
- **Starter dependencies** (Simplified POMs)
- **Production-ready features** (Actuator, metrics)

## **2. Architectural Layers**
Here's how Spring Boot structures applications:

### **A. Presentation Layer**
- **Components**: `@RestController`, `@Controller`
- **Responsibility**: Handles HTTP requests/responses
- **Example**:
  ```java
  @RestController
  public class UserController {
      @GetMapping("/users")
      public List<User> getUsers() {
          return userService.findAll();
      }
  }
  ```

### **B. Business Layer**
- **Components**: `@Service`, `@Component`
- **Responsibility**: Contains business logic
- **Example**:
  ```java
  @Service
  public class UserService {
      @Autowired
      private UserRepository repo;
      
      public List<User> findAll() {
          return repo.findAll();
      }
  }
  ```

### **C. Data Access Layer**
- **Components**: `@Repository`, Spring Data JPA
- **Responsibility**: Database interactions
- **Example**:
  ```java
  @Repository
  public interface UserRepository extends JpaRepository<User, Long> {}
  ```

### **D. Integration Layer**
- Handles external systems (REST APIs, messaging queues)
- Common annotations: `@FeignClient`, `@KafkaListener`

## **3. Core Architectural Components**

### **1. Spring Boot Starter**
- **Purpose**: Simplified dependency management
- **How it works**:
  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  ```
  - Bundles Tomcat + Spring MVC + JSON support

### **2. Auto-Configuration**
- **Mechanism**:
  1. Scans classpath
  2. Checks `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`
  3. Applies defaults
- **Example**: Adding `spring-boot-starter-data-jpa` auto-configures:
  - DataSource
  - Hibernate
  - JPA EntityManager

### **3. Embedded Server**
- **Options**: Tomcat (default), Jetty, Undertow
- **How it works**:
  ```java
  @SpringBootApplication
  public class App {
      public static void main(String[] args) {
          // Starts embedded server on port 8080
          SpringApplication.run(App.class, args);
      }
  }
  ```

### **4. Spring Boot Actuator**
- **Production monitoring**:
  ```yaml
  management:
    endpoints:
      web:
        exposure:
          include: health,info,metrics
  ```
- **Key endpoints**:
  - `/actuator/health` - Application health
  - `/actuator/metrics` - Performance metrics
  - `/actuator/env` - Environment variables

## **4. Flow of Request Processing**
1. **HTTP Request** → DispatcherServlet (Auto-configured)
2. **Routes to Controller** via `@RequestMapping`
3. **Business Logic** processed in Service layer
4. **Data Access** via Repository
5. **Response** returned through Controller

## **5. Configuration Architecture**
Spring Boot's **hierarchical** property loading order:
1. `@PropertySource` annotations
2. **application.properties** (or `.yml`)
3. OS environment variables
4. Command-line arguments

Example property override:
```properties
# application-dev.properties
server.port=8081
```

## **6. Spring Boot vs Traditional Architecture**
| Aspect | Traditional Spring | Spring Boot |
|--------|-------------------|-------------|
| **Configuration** | Manual XML/Java | Auto-configured |
| **Deployment** | WAR on external server | Self-contained JAR |
| **Dependencies** | Manual version mgmt | Starter POMs |
| **Bootstrapping** | Multiple config files | Single `@SpringBootApplication` |

## **7. Best Practices**
1. **Keep layered architecture** (Controller → Service → Repository)
2. **Use profiles** for environment-specific configs
3. **Leverage Actuator** for production monitoring
4. **Externalize configuration** (avoid hardcoding)

Great question! Spring and Spring Boot together offer a wide variety of **annotations** to make development faster, cleaner, and more modular. Here's a **comprehensive list** of the most commonly used annotations grouped by purpose:

---
## **Annotations**

## 🔹 **1. Core Spring Annotations**

| Annotation            | Purpose |
|------------------------|---------|
| `@Configuration`       | Marks a class as a source of bean definitions |
| `@Bean`                | Declares a bean manually within a configuration class |
| `@Component`           | Generic stereotype for any Spring-managed component |
| `@Service`             | Specialization of `@Component` for service layer |
| `@Repository`          | Specialization of `@Component` for persistence layer |
| `@Autowired`           | Injects dependencies automatically |
| `@Qualifier`           | Resolves conflicts when multiple beans of the same type exist |
| `@Value`               | Injects values from `application.properties` |
| `@Primary`             | Marks a bean as the default one when multiple candidates exist |
| `@Lazy`                | Initializes the bean lazily (on first use) |
| `@DependsOn`           | Specifies bean initialization order |
| `@Scope`               | Defines the scope of a bean (`singleton`, `prototype`, etc.) |

---

## 🔹 **2. Spring Boot Annotations**

| Annotation                | Purpose |
|----------------------------|---------|
| `@SpringBootApplication`   | Combines `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan` |
| `@EnableAutoConfiguration` | Enables Spring Boot’s auto-configuration feature |
| `@ComponentScan`           | Tells Spring to scan for annotated components in a package |
| `@SpringBootTest`          | Used for Spring Boot integration tests |
| `@EnableConfigurationProperties` | Binds configuration properties to Java beans |
| `@ConfigurationProperties` | Maps multiple properties from `application.properties` to a POJO |
| `@RestControllerAdvice`    | Global exception handling for REST controllers |
| `@SpringApplicationConfiguration` | Used in older Spring Boot versions for test config |

---

## 🔹 **3. Web and REST Annotations (Spring MVC)**

| Annotation          | Purpose |
|----------------------|---------|
| `@Controller`        | Marks a class as a web controller (returns views) |
| `@RestController`    | Combines `@Controller` + `@ResponseBody` (returns data, not views) |
| `@RequestMapping`    | Maps HTTP requests to controller classes/methods |
| `@GetMapping`        | Shortcut for `@RequestMapping(method = RequestMethod.GET)` |
| `@PostMapping`       | Shortcut for POST |
| `@PutMapping`        | Shortcut for PUT |
| `@DeleteMapping`     | Shortcut for DELETE |
| `@PatchMapping`      | Shortcut for PATCH |
| `@PathVariable`      | Binds a URI variable to a method parameter |
| `@RequestParam`      | Binds query parameters to method arguments |
| `@RequestBody`       | Binds the request body to a method parameter |
| `@ResponseBody`      | Sends return values directly as HTTP responses |
| `@ResponseStatus`    | Sets the HTTP status for a method |
| `@RequestHeader`     | Injects request header values |
| `@CookieValue`       | Injects cookie values |

---

## 🔹 **4. Spring Data JPA Annotations**

| Annotation          | Purpose |
|----------------------|---------|
| `@Entity`            | Marks a class as a JPA entity |
| `@Id`                | Marks the primary key |
| `@GeneratedValue`    | Specifies how the primary key is generated |
| `@Table`             | Specifies the table name |
| `@Column`            | Specifies column details |
| `@OneToOne` / `@OneToMany` / `@ManyToOne` / `@ManyToMany` | Defines relationships between entities |
| `@JoinColumn`        | Defines foreign key column |
| `@Repository`        | Marks a Spring Data repository |

---

## 🔹 **5. Spring Security Annotations**

| Annotation              | Purpose |
|--------------------------|---------|
| `@EnableWebSecurity`     | Enables Spring Security config |
| `@PreAuthorize`          | Method-level security with expressions |
| `@Secured`               | Limits access to methods to specific roles |
| `@RolesAllowed`          | Same as above (Java EE style) |
| `@AuthenticationPrincipal` | Injects the current authenticated user |

---

## 🔹 **6. Testing Annotations**

| Annotation           | Purpose |
|-----------------------|---------|
| `@SpringBootTest`     | Integration testing |
| `@WebMvcTest`         | Tests only Spring MVC components |
| `@DataJpaTest`        | Tests only JPA components |
| `@MockBean`           | Creates and injects a mock bean |
| `@TestConfiguration`  | Custom configuration for testing |

---

## 🔹 **7. Other Useful Annotations**

| Annotation              | Purpose |
|--------------------------|---------|
| `@EnableScheduling`      | Enables scheduled tasks |
| `@Scheduled`             | Defines scheduled tasks |
| `@EnableAsync`           | Enables async method execution |
| `@Async`                 | Runs a method asynchronously |
| `@EnableCaching`         | Enables caching |
| `@Cacheable`             | Caches the method’s return value |
| `@CacheEvict`            | Removes entry from cache |

Absolutely! Let’s deep-dive into each Spring Boot annotation you listed with full explanation, **when to use**, and **clear examples** for each.

---

## 🔹 1. `@SpringBootApplication`

### ✅ What It Does:
This is the **main annotation** used to bootstrap a Spring Boot application.  
It combines:
- `@Configuration` — allows the class to define beans using `@Bean`
- `@EnableAutoConfiguration` — auto-configures Spring Beans based on the classpath and properties
- `@ComponentScan` — scans for Spring components in the current package and subpackages

### 🧠 When to Use:
Always use this on your **main application class** (entry point).

### 💡 Example:
```java
@SpringBootApplication
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}
```

This eliminates the need to write 3 separate annotations manually.

---

## 🔹 2. `@EnableAutoConfiguration`

### ✅ What It Does:
Tells Spring Boot to **automatically configure beans** based on dependencies in your project (e.g., if `spring-boot-starter-web` is in the classpath, it sets up Tomcat, Jackson, etc.).

### 🧠 When to Use:
Use it when you want **auto-configuration** but don’t want to use `@SpringBootApplication`.

### 💡 Example:
```java
@Configuration
@EnableAutoConfiguration
public class MyConfig {
    public static void main(String[] args) {
        SpringApplication.run(MyConfig.class, args);
    }
}
```

---

## 🔹 3. `@ComponentScan`

### ✅ What It Does:
Tells Spring where to **look for components** (`@Component`, `@Service`, `@Repository`, etc.) to register as beans.

### 🧠 When to Use:
When your components are outside the current package of the main class. Customize it to specify the base packages.

### 💡 Example:
```java
@ComponentScan(basePackages = {"com.myapp.services", "com.myapp.controllers"})
public class AppConfig { }
```

---

## 🔹 4. `@SpringBootTest`

### ✅ What It Does:
Used for **integration testing** in Spring Boot. Loads the full application context and allows you to test components as they work together.

### 🧠 When to Use:
When you want to test the full Spring context (e.g., service with repository or controller with service).

### 💡 Example:
```java
@SpringBootTest
class UserServiceTest {

    @Autowired
    private UserService userService;

    @Test
    void testGreeting() {
        assertEquals("Hello", userService.greet());
    }
}
```

---

## 🔹 5. `@EnableConfigurationProperties`

### ✅ What It Does:
Enables support for `@ConfigurationProperties` annotated beans. Without this, Spring Boot won't map values from `application.properties`.

### 🧠 When to Use:
Use when you want to **externalize configuration** into a POJO from properties/yaml.

### 💡 Example:
```java
@SpringBootApplication
@EnableConfigurationProperties(AppConfig.class)
public class MyApp { }
```

---

## 🔹 6. `@ConfigurationProperties`

### ✅ What It Does:
Maps hierarchical configuration properties (from `application.properties` or `application.yml`) to a Java bean.

### 🧠 When to Use:
When you have **grouped or nested configuration** settings to bind to a class.

### 💡 Example:
```properties
app.name=TestApp
app.version=1.0
```

```java
@Component
@ConfigurationProperties(prefix = "app")
public class AppConfig {
    private String name;
    private String version;
    
    // Getters and Setters
}
```

---

## 🔹 7. `@RestControllerAdvice`

### ✅ What It Does:
It's a combination of `@ControllerAdvice` and `@ResponseBody`, used for **global exception handling** in REST controllers.

### 🧠 When to Use:
Use when you want to handle exceptions from **all REST endpoints** in one place.

### 💡 Example:
```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleException(Exception ex) {
        return new ResponseEntity<>("Something went wrong!", HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

---

## 🔹 8. `@SpringApplicationConfiguration` (Deprecated)

### ✅ What It Did:
Used in **older Spring Boot versions** to configure the application context for tests.

### 🧠 When to Use:
⚠️ **Deprecated** — no longer needed after Spring Boot 1.4.  
Use `@SpringBootTest` instead.

### 💡 Modern Alternative:
```java
@SpringBootTest
public class MyAppTest {
    // test methods
}
```

---

### ✅ Summary Table

| Annotation                  | Use Case |
|-----------------------------|----------|
| `@SpringBootApplication`     | Main entry point of Spring Boot app |
| `@EnableAutoConfiguration`   | Enables auto setup of beans based on dependencies |
| `@ComponentScan`             | Scans packages for Spring components |
| `@SpringBootTest`            | Full integration testing |
| `@EnableConfigurationProperties` | Enables usage of `@ConfigurationProperties` |
| `@ConfigurationProperties`   | Maps grouped config values to POJO |
| `@RestControllerAdvice`      | Centralized REST exception handling |
| `@SpringApplicationConfiguration` | 🛑 Deprecated, replaced by `@SpringBootTest` |


 
 ###  `Core Spring Annotation ` 

---

### 🔹 1. `@Configuration`

- **Purpose**: Indicates that a class contains bean definitions that should be managed by Spring's IoC container.
- **Use Case**: When you define beans manually using `@Bean`.
- **Example**:
  ```java
  @Configuration
  public class AppConfig {
      @Bean
      public MyService myService() {
          return new MyService();
      }
  }
  ```

---

### 🔹 2. `@Bean`

- **Purpose**: Declares a method that returns a Spring bean to be managed by the Spring container.
- **Use Case**: For third-party or manually configured classes that aren't annotated with `@Component`.
- **Example**:
  ```java
  @Bean
  public MyRepository myRepository() {
      return new MyRepository();
  }
  ```

---

### 🔹 3. `@Component`

- **Purpose**: A generic stereotype to register a class as a Spring-managed component.
- **Use Case**: For custom utility classes or general-purpose components.
- **Example**:
  ```java
  @Component
  public class MyUtility {
      public void doSomething() { }
  }
  ```

---

### 🔹 4. `@Service`

- **Purpose**: Specialized version of `@Component` used for service layer classes.
- **Use Case**: Use in your business logic layer.
- **Example**:
  ```java
  @Service
  public class UserService {
      public String getUser() { return "John"; }
  }
  ```

---

### 🔹 5. `@Repository`

- **Purpose**: Specialized version of `@Component` for DAO or persistence layer classes. Enables automatic exception translation.
- **Use Case**: Classes that interact with the database.
- **Example**:
  ```java
  @Repository
  public class UserRepository {
      public User findById(int id) { ... }
  }
  ```

---

### 🔹 6. `@Autowired`

- **Purpose**: Automatically injects dependent beans by type.
- **Use Case**: Inject services or repositories into your class.
- **Example**:
  ```java
  @Autowired
  private UserService userService;
  ```

---

### 🔹 7. `@Qualifier`

- **Purpose**: Resolves ambiguity when multiple beans of the same type are present.
- **Use Case**: When you have more than one bean of the same interface/class.
- **Example**:
  ```java
  @Autowired
  @Qualifier("advancedUserService")
  private UserService userService;
  ```

---

### 🔹 8. `@Value`

- **Purpose**: Injects values from properties files or environment variables.
- **Use Case**: Load config like URLs, tokens, or feature flags.
- **Example**:
  ```java
  @Value("${app.name}")
  private String appName;
  ```

---

### 🔹 9. `@Primary`

- **Purpose**: Marks a bean as the default when multiple candidates exist.
- **Use Case**: Provide a default implementation.
- **Example**:
  ```java
  @Primary
  @Bean
  public UserService userService() {
      return new DefaultUserService();
  }
  ```

---

### 🔹 10. `@Lazy`

- **Purpose**: Delays bean creation until it's actually needed.
- **Use Case**: For performance optimization or circular dependencies.
- **Example**:
  ```java
  @Lazy
  @Component
  public class HeavyComponent {
      ...
  }
  ```

---

### 🔹 11. `@DependsOn`

- **Purpose**: Specifies the initialization order of beans.
- **Use Case**: If one bean must be initialized before another.
- **Example**:
  ```java
  @Component
  @DependsOn("dataSource")
  public class DataLoader {
      ...
  }
  ```

---

### 🔹 12. `@Scope`

- **Purpose**: Defines bean scope like `singleton`, `prototype`, `request`, `session`, etc.
- **Use Case**: For stateless or stateful components.
- **Example**:
  ```java
  @Component
  @Scope("prototype")
  public class TaskProcessor {
      ...
  }
  ```
  It seems like I can’t do more advanced data analysis right now. Please try again later.

In the meantime, I can still provide you with the **detailed explanations** for each of the **Web and REST annotations** right here:

---

### 🔹 `@Controller`
- **Purpose**: Marks the class as a Spring MVC controller that returns views (like HTML pages).
- **Use**: When you're building a traditional web app.
- **Example**:
  ```java
  @Controller
  public class HomeController {
      @GetMapping("/")
      public String home() {
          return "home"; // Refers to home.html or home.jsp
      }
  }
  ```

---

### 🔹 `@RestController`
- **Purpose**: Combines `@Controller` + `@ResponseBody`. Used to build REST APIs that return data.
- **Use**: When building APIs returning JSON or XML.
- **Example**:
  ```java
  @RestController
  public class UserController {
      @GetMapping("/user")
      public User getUser() {
          return new User("John", "Doe");
      }
  }
  ```

---

### 🔹 `@RequestMapping`
- **Purpose**: Maps HTTP requests to controller classes/methods.
- **Use**: For general mappings; can specify method types, paths, headers.
- **Example**:
  ```java
  @RequestMapping("/api")
  public class ApiController {
      @RequestMapping("/hello")
      public String sayHello() {
          return "Hello!";
      }
  }
  ```

---

### 🔹 `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, `@PatchMapping`
- **Purpose**: Shorthand for `@RequestMapping(method = ...)`
- **Use**: Makes controller methods more readable.

**Examples**:
```java
@GetMapping("/users") // for GET
public List<User> getUsers() { ... }

@PostMapping("/users") // for POST
public void createUser(@RequestBody User user) { ... }

@PutMapping("/users/{id}") // for PUT
public void updateUser(@PathVariable int id, @RequestBody User user) { ... }

@DeleteMapping("/users/{id}") // for DELETE
public void deleteUser(@PathVariable int id) { ... }

@PatchMapping("/users/{id}") // for PATCH
public void patchUser(@PathVariable int id, @RequestBody Map<String, Object> updates) { ... }
```

---

### 🔹 `@PathVariable`
- **Purpose**: Binds a URI variable to a method parameter.
- **Example**:
  ```java
  @GetMapping("/users/{id}")
  public User getUser(@PathVariable int id) {
      return userService.findById(id);
  }
  ```

---

### 🔹 `@RequestParam`
- **Purpose**: Binds a query parameter to a method argument.
- **Example**:
  ```java
  @GetMapping("/search")
  public List<User> search(@RequestParam String name) {
      return userService.searchByName(name);
  }
  ```

---

### 🔹 `@RequestBody`
- **Purpose**: Binds HTTP request body to a Java object.
- **Example**:
  ```java
  @PostMapping("/users")
  public void addUser(@RequestBody User user) {
      userService.save(user);
  }
  ```

---

### 🔹 `@ResponseBody`
- **Purpose**: Sends the return value directly in the HTTP response (not as a view).
- **Example**:
  ```java
  @ResponseBody
  @GetMapping("/greet")
  public String greet() {
      return "Hello!";
  }
  ```

---

### 🔹 `@ResponseStatus`
- **Purpose**: Sets the HTTP status for a method.
- **Example**:
  ```java
  @ResponseStatus(HttpStatus.CREATED)
  @PostMapping("/users")
  public void createUser(@RequestBody User user) {
      // Save user
  }
  ```

---

### 🔹 `@RequestHeader`
- **Purpose**: Binds a request header to a method parameter.
- **Example**:
  ```java
  @GetMapping("/info")
  public String getInfo(@RequestHeader("User-Agent") String userAgent) {
      return "User-Agent: " + userAgent;
  }
  ```

---

### 🔹 `@CookieValue`
- **Purpose**: Binds a cookie value to a method parameter.
- **Example**:
  ```java
  @GetMapping("/preferences")
  public String getPrefs(@CookieValue("lang") String language) {
      return "Preferred language: " + language;
  }
  ```

Here's a complete explanation of the **Spring Data JPA annotations** you listed — with use cases, purpose, and code examples:

---

### 🔹 `@Entity`

- **Purpose**: Marks a class as a JPA entity (mapped to a database table).
- **Use Case**: Any class you want persisted in the database.
- **Example**:
  ```java
  @Entity
  public class User {
      @Id
      @GeneratedValue
      private Long id;
      private String name;
  }
  ```

---

### 🔹 `@Id`

- **Purpose**: Specifies the primary key of an entity.
- **Use Case**: Required for each JPA entity.
- **Example**:
  ```java
  @Id
  private Long id;
  ```

---

### 🔹 `@GeneratedValue`

- **Purpose**: Specifies how the primary key is generated (auto, identity, sequence).
- **Use Case**: When you want the database to generate primary keys automatically.
- **Example**:
  ```java
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;
  ```

---

### 🔹 `@Table`

- **Purpose**: Specifies the name of the table that the entity maps to.
- **Use Case**: Use when the table name is different from the class name.
- **Example**:
  ```java
  @Entity
  @Table(name = "users")
  public class User { ... }
  ```

---

### 🔹 `@Column`

- **Purpose**: Specifies the details of the column in the table.
- **Use Case**: Customize column names, nullability, length, etc.
- **Example**:
  ```java
  @Column(name = "user_name", nullable = false, length = 50)
  private String name;
  ```

---

### 🔹 `@OneToOne`

- **Purpose**: One-to-one relationship between two entities.
- **Use Case**: User has one profile.
- **Example**:
  ```java
  @OneToOne
  @JoinColumn(name = "profile_id")
  private Profile profile;
  ```

---

### 🔹 `@OneToMany`

- **Purpose**: One-to-many relationship.
- **Use Case**: A user has many orders.
- **Example**:
  ```java
  @OneToMany(mappedBy = "user")
  private List<Order> orders;
  ```

---

### 🔹 `@ManyToOne`

- **Purpose**: Many entities relate to one entity.
- **Use Case**: Many orders belong to one user.
- **Example**:
  ```java
  @ManyToOne
  @JoinColumn(name = "user_id")
  private User user;
  ```

---

### 🔹 `@ManyToMany`

- **Purpose**: Many-to-many relationship.
- **Use Case**: A student can enroll in many courses and vice versa.
- **Example**:
  ```java
  @ManyToMany
  @JoinTable(name = "student_course",
             joinColumns = @JoinColumn(name = "student_id"),
             inverseJoinColumns = @JoinColumn(name = "course_id"))
  private List<Course> courses;
  ```

---

### 🔹 `@JoinColumn`

- **Purpose**: Specifies the foreign key column.
- **Use Case**: Required in relationships to define how tables are linked.
- **Example**:
  ```java
  @ManyToOne
  @JoinColumn(name = "user_id")
  private User user;
  ```

---

### 🔹 `@Repository`

- **Purpose**: Marks a class as a DAO and enables exception translation into Spring’s DataAccessException.
- **Use Case**: On interfaces or classes that perform DB operations.
- **Example**:
  ```java
  @Repository
  public interface UserRepository extends JpaRepository<User, Long> {
      User findByName(String name);
  }
  ```

Great! Let’s summarize and clearly distinguish between **JavaBeans**, **POJOs**, **DTOs**, **DAOs**, **Value Objects**, and **Mappers**, with real-world relevance and practical usage in Spring Boot applications.

---

## 🔹 1. **POJO (Plain Old Java Object)**

### ✅ What is it?
A **POJO** is a simple Java class that doesn’t implement any special interfaces or inherit from specific classes. It’s just a plain object.

### ✅ Characteristics:
- No restrictions
- No special annotations or interfaces
- Can have any fields and methods

### ✅ Example:
```java
public class User {
    private String name;
    private int age;
    
    // Constructor, Getters, Setters
}
```

📌 **When to use**: Any time you need a simple object to carry data or logic.

---

## 🔹 2. **JavaBean**

### ✅ What is it?
A **JavaBean** is a **POJO** that follows specific conventions:
- Public no-arg constructor
- Private properties with public getters/setters
- Serializable

### ✅ Example:
```java
public class Employee implements Serializable {
    private String id;
    private String name;

    public Employee() {}  // No-arg constructor

    public String getId() { return id; }
    public void setId(String id) { this.id = id; }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
}
```

📌 **When to use**: Especially useful in frameworks (like Spring, JSP, etc.) that rely on getters/setters and object introspection.

---

## 🔹 3. **DTO (Data Transfer Object)**

### ✅ What is it?
A **DTO** is a Java object used to **transfer data** between layers (controller → service → client). DTOs contain only data (no business logic).

### ✅ Example:
```java
public class UserDTO {
    private String name;
    private String email;

    // Constructors, Getters, Setters
}
```

📌 **When to use**:
- To avoid exposing your Entity directly to the client.
- To send only the necessary data fields in the response.
- To reshape or transform data.

---

## 🔹 4. **DAO (Data Access Object)**

### ✅ What is it?
A **DAO** encapsulates the logic for accessing persistent storage (e.g., database). In Spring Boot, DAO is implemented via `@Repository` interfaces.

### ✅ Example:
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
}
```

📌 **When to use**: Always for database operations to isolate persistence logic from the rest of the application.

---

## 🔹 5. **Value Object (VO)**

### ✅ What is it?
A **VO** represents an immutable object whose equality is based on value, not identity. If all its properties are equal, the objects are considered equal.

### ✅ Characteristics:
- Immutable (fields set only in constructor)
- Equals and hashCode are based on all fields
- No setters

### ✅ Example:
```java
public final class Money {
    private final BigDecimal amount;
    private final String currency;

    public Money(BigDecimal amount, String currency) {
        this.amount = amount;
        this.currency = currency;
    }

    // Getters only, Equals & hashCode
}
```

📌 **When to use**: For entities like `Money`, `Coordinates`, `DateRange` where value matters more than identity.

---

## 🔹 6. **Mapper**

### ✅ What is it?
A **Mapper** is a utility to convert **Entity ↔ DTO**. It avoids code duplication and separates conversion logic.

### ✅ Manual Example:
```java
public class UserMapper {
    public static UserDTO toDTO(User user) {
        return new UserDTO(user.getName(), user.getEmail());
    }

    public static User toEntity(UserDTO dto) {
        User user = new User();
        user.setName(dto.getName());
        user.setEmail(dto.getEmail());
        return user;
    }
}
```

### ✅ With MapStruct:
```java
@Mapper(componentModel = "spring")
public interface UserMapper {
    UserDTO toDTO(User user);
    User toEntity(UserDTO dto);
}
```

📌 **When to use**: Always when you need to separate persistence models (entities) from response/request models (DTOs).

---

## 📌 Summary Table

| Term        | Purpose                                      | Mutable | Serializable | Example Usage                   |
|-------------|----------------------------------------------|---------|--------------|----------------------------------|
| POJO        | Basic Java class                             | Yes     | Optional     | Any simple class                |
| JavaBean    | POJO with getter/setter conventions           | Yes     | Yes          | Framework bindings              |
| DTO         | Data transport layer (no logic)              | Yes     | Yes          | Controller → Service            |
| DAO         | Database access abstraction                  | N/A     | N/A          | Repositories                    |
| VO          | Immutable value-based object                 | No      | Optional     | Money, DateRange, Coordinates   |
| Mapper      | Converts between DTOs and Entities           | N/A     | N/A          | Model mapping                   |

Great! Here’s a comprehensive and easy-to-follow breakdown of all the **Spring Data JPA** topics you mentioned — perfect for your notes:

---

## **1. Spring Data JPA**

**Spring Data JPA** is part of the larger Spring Data family. It simplifies the implementation of data access layers by:
- Eliminating boilerplate DAO code
- Providing powerful abstractions for CRUD operations
- Supporting custom queries with JPQL or native SQL

---

## **2. Entity, Repository, CrudRepository, JpaRepository**

### ➤ `@Entity`
Represents a table in the database.

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
}
```

---

### ➤ Repository Hierarchy

| Interface         | Description                              |
|-------------------|------------------------------------------|
| `Repository`      | Base interface, not used directly         |
| `CrudRepository`  | Basic CRUD operations (`save`, `findById`, `delete`) |
| `JpaRepository`   | Extends `CrudRepository`, adds pagination, sorting, and batch methods |

```java
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByName(String name);
}
```

---

## **3. JPQL and Native Queries**

### ➤ JPQL (Java Persistence Query Language)
Object-oriented query language — works with entity names & fields.

```java
@Query("SELECT u FROM User u WHERE u.email = ?1")
User findByEmail(String email);
```

---

### ➤ Native SQL
Direct SQL queries on the actual database tables.

```java
@Query(value = "SELECT * FROM users WHERE email = ?1", nativeQuery = true)
User findByEmailNative(String email);
```

---

## **4. Database Configuration**

### ➤ H2 (In-memory database, great for dev/test)

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
```

### ➤ MySQL / PostgreSQL Example

```properties
# MySQL
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.database-platform=org.hibernate.dialect.MySQLDialect
```

```properties
# PostgreSQL
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
spring.datasource.username=postgres
spring.datasource.password=admin
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
```

---

## **5. Spring Boot with Hibernate**

Hibernate is the default JPA implementation used in Spring Boot.

**Common Hibernate Properties:**

```properties
spring.jpa.show-sql=true                # show SQL in logs
spring.jpa.hibernate.ddl-auto=update    # auto-create tables (none, validate, update, create)
spring.jpa.properties.hibernate.format_sql=true
```

Hibernate maps Java objects to relational DB tables and handles:

- Entity lifecycle
- Query translation (JPQL to SQL)
- Lazy vs Eager fetching

---

## **6. DTOs and Model Mapping**

### ➤ Why Use DTOs (Data Transfer Objects)?
- Avoid exposing full entity structure
- Improve performance by fetching only needed data
- Format/transform data before sending to frontend

```java
public class UserDTO {
    private String name;
    private String email;
}
```

### ➤ Mapping Entity to DTO (Manual)

```java
UserDTO dto = new UserDTO();
dto.setName(user.getName());
dto.setEmail(user.getEmail());
```

### ➤ Using ModelMapper (Optional)

```java
ModelMapper modelMapper = new ModelMapper();
UserDTO dto = modelMapper.map(user, UserDTO.class);
```

Add dependency:
```xml
<dependency>
    <groupId>org.modelmapper</groupId>
    <artifactId>modelmapper</artifactId>
    <version>3.1.0</version>
</dependency>
```

---

## ✅ Summary Table

| Concept              | Key Role                                      |
|----------------------|-----------------------------------------------|
| `@Entity`            | Maps Java class to DB table                   |
| `JpaRepository`      | Provides CRUD + pagination + custom queries   |
| JPQL                 | Object-oriented querying                      |
| Native Query         | Direct SQL querying                           |
| `application.properties` | DB config, dialect, Hibernate options     |
| DTO                  | Transfers specific data to avoid entity leaks |
| ModelMapper          | Auto-map between Entity & DTO                 |

Here are the **Spring Security notes** based on your `SecurityConfig` class:

---
## 🛡️ Spring Security

### `SecurityConfig.java`

### 📦 Annotations Used
- `@Configuration`: Marks the class as a source of bean definitions.
- `@EnableWebSecurity`: Enables Spring Security for web security configuration.
- `@EnableMethodSecurity`: Enables method-level security annotations like `@PreAuthorize`, `@Secured`, etc.
- `@RequiredArgsConstructor`: Generates a constructor with required final fields (via Lombok).

---

### 🔐 `SecurityFilterChain` Bean
```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception
```
- **Disables CSRF**: `.csrf().disable()`
- **Authorizes HTTP Requests**:
  - `/api/v1/register`, `/login`, `/refresh-token`: **publicly accessible**
  - `/api/v1/admin/**`: **accessible to ADMIN**
  - `/api/v1/user/**`: **accessible to USER or ADMIN**
  - Any other request: **authentication required**
- **Exception Handling**:
  - Custom `AccessDeniedHandler` is configured
- **Session Management**:
  - `SessionCreationPolicy.STATELESS`: stateless session (important for JWT-based auth)
- **JWT Filter Added**:
  - `jwtFilter` is added **before** `UsernamePasswordAuthenticationFilter`

---

### 🔐 `AuthenticationProvider` Bean
```java
@Bean
public AuthenticationProvider authenticationProvider()
```
- Uses `DaoAuthenticationProvider` for username/password authentication.
- Injects `MyUserDetailsService` to load user data.
- Uses `BCryptPasswordEncoder` to encode passwords.
- Adds `SimpleAuthorityMapper` to **normalize roles** (like adding `ROLE_` prefix).

---

### 🔐 `AuthenticationManager` Bean
```java
@Bean
public AuthenticationManager authenticationManager(AuthenticationConfiguration configuration)
```
- Returns the `AuthenticationManager` built from Spring's auto-configured `AuthenticationConfiguration`.

---

### 🏷️ `RoleHierarchy` Bean
```java
@Bean
public RoleHierarchyImpl roleHierarchy()
```
- Defines role inheritance:
  - `ROLE_ADMIN > ROLE_USER`: Users with `ADMIN` automatically get `USER` permissions.

---

### 🧩 Autowired Dependencies
- `JwtFilter`: Custom filter that validates JWT token.
- `CustomAccessDeniedHandler`: Custom handler when access is denied.
- `MyUserDetailsService`: Loads user-specific data for authentication.

---
Here's a **clear and detailed breakdown** of both `AuthenticationManager` and `AuthenticationProvider` in Spring Security:

---

## 🔐 `AuthenticationManager`

### ✅ What it is:
- The **main interface responsible for user authentication** in Spring Security.
- It receives an `Authentication` object and returns a fully authenticated `Authentication` object if the credentials are valid.

### ✅ Purpose:
- To **coordinate** the authentication process.
- Delegates the actual verification work to one or more `AuthenticationProvider`s.

### ✅ Key Method:
```java
Authentication authenticate(Authentication authentication) throws AuthenticationException;
```

### ✅ Example Use:
In your login controller:
```java
Authentication auth = authenticationManager.authenticate(
    new UsernamePasswordAuthenticationToken(username, password)
);
```

---

## 🔌 `AuthenticationProvider`

### ✅ What it is:
- A **strategy interface** used by `AuthenticationManager` to **verify user credentials**.
- Contains the actual logic to validate the credentials and load user data.

### ✅ Purpose:
- To perform specific **authentication mechanisms** (e.g., username/password, OTP, etc.).
- You can register **multiple providers** (e.g., for different login types).

### ✅ Common Implementation:
```java
DaoAuthenticationProvider
```
- Uses a `UserDetailsService` to load the user.
- Uses a `PasswordEncoder` to check the password.

### ✅ Key Methods:
```java
Authentication authenticate(Authentication authentication) throws AuthenticationException;
boolean supports(Class<?> authentication);
```

---

## 🔄 Internal Flow Between Them

1. The login request reaches the controller.
2. You call `authenticationManager.authenticate(...)`.
3. `AuthenticationManager` loops through all registered `AuthenticationProvider`s.
4. The appropriate `AuthenticationProvider`:
   - Loads user data via `UserDetailsService`
   - Verifies password using `PasswordEncoder`
5. If successful → returns a fully authenticated object.
6. Spring Security stores it in the **SecurityContext**.

---

## 🔧 Analogy

- **AuthenticationManager** = a **gatekeeper** that checks all keys.
- **AuthenticationProvider** = a **key checker** that verifies if a particular key (credential) is valid.



### ✅ `SecurityConfig.java` (Improved Version)
```java
package com.rabbani.spring_security.config;

import com.rabbani.spring_security.exception.CustomAccessDeniedHandler;
import com.rabbani.spring_security.filter.JwtFilter;
import com.rabbani.spring_security.service.MyUserDetailsService;
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.access.hierarchicalroles.RoleHierarchyImpl;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.dao.DaoAuthenticationProvider;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.core.authority.mapping.SimpleAuthorityMapper;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
@EnableMethodSecurity
@RequiredArgsConstructor
public class SecurityConfig {

    private final JwtFilter jwtFilter;
    private final CustomAccessDeniedHandler accessDeniedHandler;
    private final MyUserDetailsService userDetailsService;

    /**
     * Main security configuration.
     * - Disables CSRF for APIs.
     * - Configures public and protected endpoints.
     * - Uses stateless session for JWT.
     * - Adds custom JWT filter before Spring’s default auth filter.
     */
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/v1/register", "/api/v1/login", "/api/v1/refresh-token").permitAll()
                .requestMatchers("/api/v1/admin/**").hasRole("ADMIN")
                .requestMatchers("/api/v1/user/**").hasAnyRole("USER", "ADMIN")
                .anyRequest().authenticated()
            )
            .exceptionHandling().accessDeniedHandler(accessDeniedHandler)
            .and()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);

        http.addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }

    /**
     * Authentication provider for Spring Security.
     * - Uses custom UserDetailsService.
     * - Encrypts passwords using BCrypt.
     * - Converts all roles to uppercase without prefix (via SimpleAuthorityMapper).
     */
    @Bean
    public AuthenticationProvider authenticationProvider() {
        DaoAuthenticationProvider provider = new DaoAuthenticationProvider();
        provider.setUserDetailsService(userDetailsService);
        provider.setPasswordEncoder(new BCryptPasswordEncoder());
        provider.setAuthoritiesMapper(new SimpleAuthorityMapper());
        return provider;
    }

    /**
     * Retrieves the AuthenticationManager from the context.
     */
    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration config) throws Exception {
        return config.getAuthenticationManager();
    }

    /**
     * Optional: Role hierarchy to allow ADMIN to have USER privileges.
     */
    @Bean
    public RoleHierarchyImpl roleHierarchy() {
        RoleHierarchyImpl roleHierarchy = new RoleHierarchyImpl();
        roleHierarchy.setHierarchy("ROLE_ADMIN > ROLE_USER");
        return roleHierarchy;
    }
}
```


### ✅ `JwtFilter.java` (With Multi-Line Comments)

```java
package com.rabbani.spring_security.filter;

import java.io.IOException;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;

import com.rabbani.spring_security.service.JwtService;
import com.rabbani.spring_security.service.MyUserDetailsService;

import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

@Component
public class JwtFilter extends OncePerRequestFilter {

    @Autowired
    private JwtService jwtService;  // Service to extract and validate JWT

    @Autowired
    private MyUserDetailsService userDetailsService;  // Service to load user from DB

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {

        /*
         * STEP 1: Extract the token from the Authorization header
         * The token must be prefixed with "Bearer "
         */
        String token = request.getHeader("Authorization");

        /*
         * STEP 2: Validate the format of the token
         * If it exists and starts with "Bearer ", extract the raw token string
         */
        if (token != null && token.startsWith("Bearer ")) {
            token = token.substring(7); // Remove "Bearer " prefix

            /*
             * STEP 3: Extract the username (subject) from the token
             */
            String username = jwtService.extractUsername(token);

            /*
             * STEP 4: Proceed only if:
             * - username is present
             * - user is not already authenticated in the SecurityContext
             */
            if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {

                /*
                 * STEP 5: Load user details from DB (using UserDetailsService)
                 */
                UserDetails userDetails = userDetailsService.loadUserByUsername(username);

                /*
                 * STEP 6: Validate the JWT token:
                 * - Check signature
                 * - Check expiration
                 * - Check subject
                 */
                if (jwtService.validateToken(token, userDetails)) {

                    /*
                     * STEP 7: If valid, create authentication object with roles
                     * and set it in the SecurityContext
                     */
                    UsernamePasswordAuthenticationToken authToken =
                        new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());

                    SecurityContextHolder.getContext().setAuthentication(authToken);
                }
            }
        }

        /*
         * STEP 8: Continue processing the rest of the filters
         */
        filterChain.doFilter(request, response);
    }
}
```

---

### ✅ Summary (In Short)

- **Purpose**: Authenticate user requests by validating JWT tokens.
- **Runs Once**: Uses `OncePerRequestFilter` to ensure it's executed once per request.
- **Authorization Header**: Looks for `Bearer <token>` format.
- **Validation**: Extracts username, loads user from DB, validates token, and sets authentication.
- **Integration**: Registered in the security config before `UsernamePasswordAuthenticationFilter`.


### ✅ `JwtService.java` (With Multi-Line Comments)

```java
package com.rabbani.spring_security.service;

import java.nio.charset.StandardCharsets;
import java.security.Key;
import java.util.Date;
import java.util.function.Function;

import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Service;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import io.jsonwebtoken.io.Decoders;
import io.jsonwebtoken.security.Keys;

@Service
public class JwtService {

    // Secret key used to sign the JWT (should ideally be stored securely, not hardcoded)
    private static final String SECRET_KEY = "VerySecretKeyForJwtSigningThatIsVeryLongToBeSecure12345";

    /*
     * Returns a HMAC-SHA key generated from the secret key string.
     * This key is used to sign and validate the JWT.
     */
    private Key getSigningKey() {
        byte[] keyBytes = SECRET_KEY.getBytes(StandardCharsets.UTF_8);
        return Keys.hmacShaKeyFor(keyBytes);
    }

    /*
     * Generates a JWT token for the given username.
     * - Sets subject as username
     * - Sets current timestamp as issue time
     * - Sets expiry time to 1 hour from now
     * - Signs the token with the secret key and HS256 algorithm
     */
    public String generateToken(String username) {
        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date(System.currentTimeMillis()))
                .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60)) // 1 hour
                .signWith(getSigningKey(), SignatureAlgorithm.HS256)
                .compact();
    }

    /*
     * Validates the token by:
     * - Extracting the username and comparing with UserDetails
     * - Ensuring the token is not expired
     */
    public Boolean validateToken(String token, UserDetails userDetails) {
        final String extractedUsername = extractUsername(token);
        return (extractedUsername.equals(userDetails.getUsername()) && !isTokenExpired(token));
    }

    /*
     * Extracts the username (subject) from the token.
     */
    public String extractUsername(String token) {
        return extractClaim(token, Claims::getSubject);
    }

    /*
     * Extracts a specific claim from the token using a claimsResolver function.
     * Can be used to extract subject, expiration, roles, etc.
     */
    private <T> T extractClaim(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = extractAllClaims(token);
        return claimsResolver.apply(claims);
    }

    /*
     * Parses the JWT and extracts all claims from the body.
     */
    private Claims extractAllClaims(String token) {
        return Jwts.parserBuilder()
                .setSigningKey(getSigningKey())
                .build()
                .parseClaimsJws(token)
                .getBody();
    }

    /*
     * Checks whether the token is expired by comparing expiration date with the current date.
     */
    private boolean isTokenExpired(String token) {
        return extractAllClaims(token).getExpiration().before(new Date());
    }
}
```

---

### ✅ Summary

| Function                | Description                                                                 |
|-------------------------|-----------------------------------------------------------------------------|
| `generateToken()`       | Creates a JWT token with a 1-hour expiry and signs it with HS256 algorithm.|
| `validateToken()`       | Confirms the token is valid (matches user and not expired).                |
| `extractUsername()`     | Gets the subject (username) from the token.                                |
| `extractClaim()`        | Generic method to extract any field from token claims.                     |
| `extractAllClaims()`    | Parses the token and returns all its claims.                               |
| `isTokenExpired()`      | Checks if the token's expiry time is before now.                           |

---

### ✅ `MyUserDetailsService.java` (With Multi-line Comments)

```java
package com.rabbani.spring_security.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import com.rabbani.spring_security.model.User;
import com.rabbani.spring_security.repository.UserRepository;

/*
 * This service implements Spring Security's UserDetailsService interface.
 * It is used by the authentication provider to fetch user details from the database
 * using the username provided during login.
 */
@Service
public class MyUserDetailsService implements UserDetailsService {

    @Autowired
    private UserRepository userRepo;

    /*
     * This method is called by Spring Security during the authentication process.
     * It loads the user from the database using the username.
     * If the user is found, it wraps the user entity in a custom UserDetails implementation (UserPrincipal).
     * If not found, it throws UsernameNotFoundException.
     */
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepo.findByUsername(username);
        if (user == null) {
            throw new UsernameNotFoundException("User not found");
        }
        return new UserPrincipal(user);
    }
}
```

---

### ✅ Summary

| Element                       | Purpose                                                                 |
|------------------------------|-------------------------------------------------------------------------|
| `UserDetailsService`         | Spring Security interface to fetch user details for authentication.     |
| `loadUserByUsername()`       | Method that finds user by username and returns a `UserDetails` object.  |
| `UserRepository`             | Spring Data JPA interface used to access user data from the database.   |
| `UserPrincipal`              | Custom class implementing `UserDetails` to wrap your domain `User`.     |
| `@Service`                   | Marks this class as a Spring bean to be managed by Spring container.    |

    

---

### ✅ `UserPrincipal.java` (With Multi-line Comments)

```java
package com.rabbani.spring_security.service;

import java.util.Collection;
import java.util.Collections;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import com.rabbani.spring_security.model.User;

/*
 * UserPrincipal is a custom implementation of Spring Security's UserDetails interface.
 * It acts as a wrapper around your User entity and tells Spring Security how to extract
 * authentication and authorization data from your User object.
 */
public class UserPrincipal implements UserDetails {

    private final User user;

    // Constructor takes your domain User object and stores it for future reference
    public UserPrincipal(User user) {
        this.user = user;
    }

    /*
     * This method returns the roles/authorities of the user.
     * Spring Security expects roles to be prefixed with "ROLE_".
     * So if the user role is "ADMIN", this returns "ROLE_ADMIN".
     */
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return Collections.singleton(new SimpleGrantedAuthority("ROLE_" + user.getRole()));
    }

    /*
     * Returns the user's password from the User entity.
     * Used for authentication matching.
     */
    @Override
    public String getPassword() {
        return user.getPassword();
    }

    /*
     * Returns the username (used as the login identifier).
     */
    @Override
    public String getUsername() {
        return user.getUsername();
    }

    /*
     * The following four methods are used to determine the status of the user account.
     * Returning true means the account is valid and active.
     */

    @Override
    public boolean isAccountNonExpired() {
        return true;  // Account is not expired
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;  // Account is not locked
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;  // Password credentials are valid (not expired)
    }

    @Override
    public boolean isEnabled() {
        return true;  // Account is enabled
    }
}
```

---

### ✅ Summary

| Method                          | Purpose                                                                 |
|----------------------------------|-------------------------------------------------------------------------|
| `getAuthorities()`              | Returns the user role (e.g., `ROLE_USER`, `ROLE_ADMIN`).                |
| `getUsername()` / `getPassword()` | Extracts credentials from your `User` entity.                          |
| `isAccountNonExpired()`         | Always returns `true`; can be enhanced for expiration logic.           |
| `isAccountNonLocked()`          | Always returns `true`; override if locking logic is needed.            |
| `isCredentialsNonExpired()`     | Always `true`; can be extended for password aging policies.            |
| `isEnabled()`                   | Always `true`; override for soft-delete or deactivation support.       |


## 🔁 **Spring Security JWT Authentication – Full Flow**

---

### ⚙️ 1. `SecurityConfig.java` – Main Spring Security Configuration

**Purpose:**  
- Disables CSRF (for APIs)
- Configures endpoints access (public, admin-only, etc.)
- Adds `JwtFilter` before the default Spring Security filter
- Sets session policy to **stateless** (since we're using JWT)
- Registers custom authentication and role hierarchy beans

**Key Points:**
- `SecurityFilterChain` defines the core security logic.
- `authenticationProvider()` sets up `DaoAuthenticationProvider` using your own `UserDetailsService`.
- `authenticationManager()` allows Spring Security to use this provider to authenticate users.

---

### 📤 2. **Login Request Flow**  
(Usually POST `/api/v1/login` endpoint)

1. The user sends their `username` and `password`.
2. You authenticate the credentials using the `AuthenticationManager`.
3. If valid, a **JWT token** is generated using `JwtService`.
4. The token is returned to the client for future requests.

---

### 🧠 3. `JwtService.java` – JWT Token Utility

**Purpose:**  
Handles all JWT operations:
- **Generate Token** (`generateToken`)
- **Extract Username** from token (`extractUsername`)
- **Validate Token** (`validateToken`)
- **Parse Claims** (token body)

**Uses:**  
- `io.jsonwebtoken` (JJWT library)
- A secret signing key to sign and verify tokens.

---

### 🧹 4. `JwtFilter.java` – JWT Filter (Runs for Each Request)

**Purpose:**  
Intercepts every HTTP request and:
1. Extracts the JWT token from the `Authorization` header.
2. Validates the token.
3. Loads the user using `MyUserDetailsService`.
4. Sets the user into `SecurityContextHolder` (Spring's internal auth context).

**Without this, Spring wouldn't know who is making the request!**

---

### 👤 5. `MyUserDetailsService.java` – Custom User Fetcher

**Purpose:**  
Implements `UserDetailsService`, the core interface used by Spring to fetch user details.

**How it works:**
- Fetches user from the database using `UserRepository`.
- Wraps it into `UserPrincipal`, which is a Spring-compatible format.

---

### 🧾 6. `UserPrincipal.java` – Adapter Between Your User and Spring

**Purpose:**  
Spring Security needs a class that implements `UserDetails`.

**Role:**
- Exposes user's `username`, `password`, and `roles`
- Makes your `User` model usable by Spring's auth mechanism

---

### 🧑 7. `User.java` – Your Entity

**Not shown**, but likely a JPA entity with fields:
```java
private String username;
private String password;
private String role;
```

---

### 🗃️ 8. `UserRepository.java` – DB Interaction

**Likely contains:**
```java
User findByUsername(String username);
```

This allows you to retrieve a user by username for authentication.

---

### 🔐 9. AuthenticationProvider & AuthenticationManager

- `DaoAuthenticationProvider` uses `MyUserDetailsService` and `BCryptPasswordEncoder` to validate login.
- `AuthenticationManager` delegates auth to the provider.

---

### ✅ 10. Role Hierarchy (Optional but Implemented)

Defined in `SecurityConfig`:
```java
ROLE_ADMIN > ROLE_USER
```

This means any user with `ROLE_ADMIN` also has all permissions of `ROLE_USER`.

---

## 🔄 Request Lifecycle Flow

```
[Login Request] ➝ Controller ➝ AuthenticationManager ➝ MyUserDetailsService ➝ DB
                ➝ JWT Created by JwtService ➝ Token sent to client

[Subsequent Requests] ➝ JwtFilter ➝ Token Validated ➝ User loaded from DB ➝ Spring Security Context updated
                      ➝ Controller ➝ Authorized/Denied based on roles
```

---

## 📝 Summary

| Component               | Purpose                                                |
|------------------------|--------------------------------------------------------|
| `SecurityConfig`        | Configures Spring Security and JWT filter              |
| `JwtFilter`             | Validates token on every request                       |
| `JwtService`            | Generates and validates JWT                            |
| `MyUserDetailsService`  | Loads user from DB                                     |
| `UserPrincipal`         | Adapts your `User` to Spring's `UserDetails`          |
| `AuthenticationManager` | Authenticates user using the provider                 |
| `DaoAuthenticationProvider` | Validates credentials and loads user from service  |
| `RoleHierarchyImpl`     | Allows role inheritance (ADMIN > USER)                |

In **Spring Boot**, **profiles** are a way to segregate parts of your application configuration and make it only available in certain environments (e.g., development, testing, production).

---

### 🔹 What is a Spring Profile?

A **profile** in Spring Boot allows you to define different configurations for different environments.

You can annotate beans or specify configurations in property files that should only be active when a specific profile is selected.

---

### 🔹 Common Use Cases

* Use different databases in **dev**, **test**, and **prod**
* Change logging levels per environment
* Enable/disable features conditionally

---

### 🔹 How to Define a Profile

#### 1. **application.properties/yml** files

Spring Boot will load these based on the active profile.

```properties
# application-dev.properties
server.port=8081
spring.datasource.url=jdbc:h2:mem:devdb

# application-prod.properties
server.port=80
spring.datasource.url=jdbc:mysql://prod-db-server/db
```

#### 2. **application.yml** example

```yaml
spring:
  profiles:
    active: dev

---

spring:
  config:
    activate:
      on-profile: dev
  datasource:
    url: jdbc:h2:mem:devdb

---

spring:
  config:
    activate:
      on-profile: prod
  datasource:
    url: jdbc:mysql://prod-db-server/db
```

---

### 🔹 Activating a Profile

#### 1. **In application.properties**

```properties
spring.profiles.active=dev
```

#### 2. **As a command-line argument**

```bash
java -jar myapp.jar --spring.profiles.active=prod
```

#### 3. **As an environment variable**

```bash
SPRING_PROFILES_ACTIVE=prod
```

#### 4. **In application code**

```java
@Profile("dev")
@Bean
public DataSource devDataSource() {
    // return dev-specific bean
}
```

---

### 🔹 Profile-specific Beans

```java
@Configuration
public class AppConfig {

    @Bean
    @Profile("dev")
    public DataSource devDataSource() {
        return new HikariDataSource(); // dev config
    }

    @Bean
    @Profile("prod")
    public DataSource prodDataSource() {
        return new HikariDataSource(); // prod config
    }
}
```

---

### 🔹 Checking Active Profile at Runtime

```java
@Autowired
private Environment env;

public void printActiveProfiles() {
    System.out.println(Arrays.toString(env.getActiveProfiles()));
}
```

### 🔹 **Why Use Spring Profiles?**

In real-world applications, different environments need different configurations:

| Environment | Example Config                              |
| ----------- | ------------------------------------------- |
| Development | In-memory DB (H2), debug logging            |
| Testing     | Test stubs, test DB                         |
| Production  | Real DB, external services, secure settings |

Using profiles, you can easily **switch between environments** without changing code.

---

### 🔹 **How to Define and Use Spring Profiles**

#### 1. **Define Profiles in Configuration Files**

Spring allows separate `application-{profile}.properties` or `.yml` files for each profile:

```properties
# application-dev.properties
spring.datasource.url=jdbc:h2:mem:devdb
spring.jpa.show-sql=true
```

```properties
# application-prod.properties
spring.datasource.url=jdbc:mysql://prod-db-url/mydb
spring.jpa.show-sql=false
```

---

#### 2. **Activate a Profile**

✅ You can activate a profile in several ways:

* **application.properties**:

  ```properties
  spring.profiles.active=dev
  ```

* **Command Line**:

  ```bash
  java -jar myapp.jar --spring.profiles.active=prod
  ```

* **Environment Variable**:

  ```bash
  SPRING_PROFILES_ACTIVE=prod
  ```

---

#### 3. **Profile-Specific Beans**

You can annotate beans or configuration classes to load only under certain profiles:

```java
@Profile("dev")
@Bean
public DataSource h2DataSource() {
    return new EmbeddedDatabaseBuilder()
        .setType(EmbeddedDatabaseType.H2)
        .build();
}
```

This bean will only be created if the `dev` profile is active.

---

#### 4. **@Profile on Classes**

```java
@Profile("prod")
@Configuration
public class ProdConfiguration {
    // Beans for production
}
```

---

### 🔹 How Spring Resolves Profiles

* First, `application.properties` is read.
* Then, `application-{profile}.properties` overrides it if `spring.profiles.active` is set.
* Order of resolution:

  1. `application.properties`
  2. `application-{profile}.properties`
  3. Command-line arguments
  4. Environment variables

---

### 🔹 Best Practices

* Use **`default` profile** for fallback configuration.
* Avoid hardcoding environment-specific values in code.
* Keep secrets (like DB passwords) in secure configuration management systems, not in `application-*.properties`.

---

### 🔹 Real-world Use Case Example

Imagine your Spring Boot app connects to different databases based on the environment. Instead of using `if` conditions, just define:

```yaml
# application-dev.yml
spring:
  datasource:
    url: jdbc:h2:mem:testdb
```

```yaml
# application-prod.yml
spring:
  datasource:
    url: jdbc:mysql://prod-db/mydb
```

And switch environments with:

```bash
--spring.profiles.active=dev
```

---
Here's a clean and professional explanation of how to **read properties in Spring Boot applications**, covering all three main approaches, including pros, use-cases, and examples:

---

## 🔧 **How to Read Properties in Spring Boot**

Spring Boot allows reading values from property files (`application.properties` or `application.yml`) in three primary ways:

---

### 1️⃣ **Using `@Value` Annotation**
This is the most straightforward way to inject a **single property** value directly into a field.

#### ✅ Use Case:
Injecting individual configuration values like strings, integers, or booleans.

#### 🧠 Syntax:
```java
@Value("${property.name}")
private String propertyValue;
```

#### 🧪 Example:
```properties
app.title=Spring Boot Application
```

```java
@Component
public class MyComponent {

    @Value("${app.title}")
    private String appTitle;

    public void printTitle() {
        System.out.println(appTitle);
    }
}
```

#### ⚠️ Limitation:
- Not suitable for grouping related configuration.
- Hard to refactor (property keys are hardcoded).

---

### 2️⃣ **Using `Environment` Interface**
This gives **programmatic access** to properties and is useful when you want more control or conditional logic.

#### ✅ Use Case:
Use when you want to access properties dynamically or conditionally.

#### 🧠 Syntax:
```java
@Autowired
private Environment environment;

String value = environment.getProperty("property.name");
```

#### 🧪 Example:
```java
@Component
public class MyComponent {

    @Autowired
    private Environment environment;

    public void printTitle() {
        String title = environment.getProperty("app.title");
        System.out.println(title);
    }
}
```

#### ✔️ Pros:
- Flexible, dynamic access
- Can be used in utility classes or logic blocks

---

### 3️⃣ **Using `@ConfigurationProperties`**
This is the **recommended** approach for binding a group of related properties into a POJO (Plain Old Java Object).

#### ✅ Use Case:
When you have multiple related configuration properties, such as database config, mail server, etc.

#### 🧠 Syntax:
```java
@ConfigurationProperties(prefix = "app")
@Component
public class AppConfig {
    private String title;
    private String version;
    // getters and setters
}
```

```properties
app.title=Spring Boot Application
app.version=1.0.0
```

#### 🧪 Example:
```java
@Component
@ConfigurationProperties(prefix = "app")
public class AppConfig {

    private String title;
    private String version;

    // getters and setters

    public void printAppInfo() {
        System.out.println("Title: " + title);
        System.out.println("Version: " + version);
    }
}
```

#### ✔️ Pros:
- Type-safe
- Easy to group and refactor
- Easy to validate using `@Validated`

---

### 🧠 Summary Table:

| Approach                  | When to Use                       | Pros                          | Limitation                   |
|--------------------------|-----------------------------------|-------------------------------|------------------------------|
| `@Value`                 | For single property injection     | Simple, quick                 | Hardcoded keys               |
| `Environment`            | Dynamic/conditional property access | Flexible, programmatic        | Verbose, not type-safe       |
| `@ConfigurationProperties` | Grouped configuration             | Type-safe, organized, reusable | Needs additional POJO setup  |

---

Let's break this down and explain it clearly with context.

---

## ✅ **Understanding `@EnableConfigurationProperties` and `@ConfigurationProperties`**

---

### 🔹 1. **`@ConfigurationProperties(prefix = "cards")`**

This annotation is used to **bind external configuration properties (like from `application.properties` or `application.yml`)** to a Java class.

#### 🔸 Example Configuration (`application.yml`):

```yaml
cards:
  contact-info:
    email: support@cards.com
    phone: 1234567890
```

#### 🔸 Corresponding DTO (POJO):

```java
@Data
@ConfigurationProperties(prefix = "cards.contact-info")
public class CardsContactInfoDto {
    private String email;
    private String phone;
}
```

* The fields in the class map to the values under `cards.contact-info`.
* The `prefix = "cards.contact-info"` tells Spring where to start mapping.

---

### 🔹 2. **`@EnableConfigurationProperties`**

This enables support for `@ConfigurationProperties`-annotated beans. It is used to **register the DTO class as a Spring bean**.

#### 🔸 Example Usage:

```java
@EnableConfigurationProperties(value = {CardsContactInfoDto.class})
@Configuration
public class CardsConfig {
    // Now CardsContactInfoDto is available as a bean
}
```

* This means Spring will read properties and create the `CardsContactInfoDto` bean, injecting values from the config.
* Without this annotation, Spring Boot won’t know to bind and manage that DTO.

---

## ✅ Summary

| Annotation                                      | Purpose                                                        |
| ----------------------------------------------- | -------------------------------------------------------------- |
| `@ConfigurationProperties(prefix = "cards")`    | Binds external config under `cards.*` to a class               |
| `@EnableConfigurationProperties({Class.class})` | Registers the class as a Spring-managed bean to use the config |

