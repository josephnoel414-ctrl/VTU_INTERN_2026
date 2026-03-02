

# 📘 `user_auth_demo`

### Simple User Authentication using Spring Boot + MySQL

---

## 📌 Objective

Demonstrate a **basic user authentication flow** using:

* Spring Boot
* REST API
* MySQL
* Password hashing (BCrypt)

> This is intentionally **simple** (no JWT yet) to help students clearly understand the **core authentication logic**.

---

## 🗂 Project Structure

![Image](https://miro.medium.com/0%2AzScVClCCjn6jHSSC.gif)

![Image](https://substackcdn.com/image/fetch/%24s_%21hnmN%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F610c2bdd-9f59-4239-85cf-c4aa40434c46_1200x1487.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AMMNZpcA0nd387572sAhteg.jpeg)

```text
user_auth_demo
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com.demo.userauth
│   │   │       ├── UserAuthDemoApplication.java
│   │   │       ├── controller
│   │   │       │   └── AuthController.java
│   │   │       ├── service
│   │   │       │   └── AuthService.java
│   │   │       ├── repository
│   │   │       │   └── UserRepository.java
│   │   │       ├── entity
│   │   │       │   └── User.java
│   │   │       ├── dto
│   │   │       │   └── LoginRequest.java
│   │   │       └── config
│   │   │           └── AppConfig.java
│   │   └── resources
│   │       └── application.properties
│   └── test
├── pom.xml
```

---

## 🗄 Database Setup (MySQL)

```sql
CREATE DATABASE user_auth_db;
```

---

## ⚙️ `application.properties`

```properties
server.port=8080

spring.datasource.url=jdbc:mysql://localhost:3306/user_auth_db
spring.datasource.username=root
spring.datasource.password=yourpassword

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
```

---

## 📦 `pom.xml` (Required Dependencies)

```xml
<dependencies>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <scope>runtime</scope>
    </dependency>

    <!-- For Password Encoding -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

</dependencies>
```

---

## 🚀 Main Class

### `UserAuthDemoApplication.java`

```java
@SpringBootApplication
public class UserAuthDemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserAuthDemoApplication.class, args);
    }
}
```

---

## 🔐 Password Encoder Config

### `config/AppConfig.java`

```java
@Configuration
public class AppConfig {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

---

## 🧱 User Entity

### `entity/User.java`

```java
@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;
    private String password;
    private String role;

    // getters and setters
}
```

---

## 🗃 Repository Layer

### `repository/UserRepository.java`

```java
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByUsername(String username);
}
```

---

## 📩 Login DTO

### `dto/LoginRequest.java`

```java
public class LoginRequest {
    private String username;
    private String password;

    // getters and setters
}
```

---

## 🧠 Service Layer (Authentication Logic)

### `service/AuthService.java`

```java
@Service
public class AuthService {

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private PasswordEncoder passwordEncoder;

    public String login(LoginRequest request) {

        User user = userRepository.findByUsername(request.getUsername())
                .orElseThrow(() -> new RuntimeException("User not found"));

        if (passwordEncoder.matches(request.getPassword(), user.getPassword())) {
            return "LOGIN SUCCESS";
        } else {
            throw new RuntimeException("INVALID PASSWORD");
        }
    }
}
```

---

## 🌐 Controller Layer

### `controller/AuthController.java`

```java
@RestController
@RequestMapping("/auth")
public class AuthController {

    @Autowired
    private AuthService authService;

    @PostMapping("/login")
    public ResponseEntity<String> login(@RequestBody LoginRequest request) {
        return ResponseEntity.ok(authService.login(request));
    }
}
```

---

## 👤 Insert Sample User (One-Time)

You can insert directly into DB:

```sql
INSERT INTO users(username, password, role)
VALUES (
  'admin',
  '$2a$10$7aQKkGZ9m6GzZK5XHk0k5e2X6QvYF9YxK3CjFZgL4KkzjE8sD6x4a',
  'ADMIN'
);
```

> Password = `admin123` (BCrypt)

---

## 🧪 Testing via Postman

**POST**

```
http://localhost:8080/auth/login
```

**Body (JSON):**

```json
{
  "username": "admin",
  "password": "admin123"
}
```

**Response:**

```
LOGIN SUCCESS
```

---

## 🎓 What Students Learn

* Authentication flow
* Password hashing
* Controller → Service → Repository
* Real database integration
* Why passwords are never stored in plain text

