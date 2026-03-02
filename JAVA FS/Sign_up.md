Below is a **clear, classroom-friendly step-by-step flow** to implement **User Sign-Up (Registration)** using **Spring Boot + MySQL**.
This assumes **Spring Boot is already running** (as you mentioned earlier).

---

## Overall Sign-Up Flow (Big Picture)

![Image](https://media2.dev.to/dynamic/image/width%3D800%2Cheight%3D%2Cfit%3Dscale-down%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F5f40bqgmv6t959orrcze.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AIBE7JQ2wRZfkp87PP-_U-w.jpeg)

![Image](https://www.codejava.net/images/articles/frameworks/springboot/register-login/user_registration_form.png)

---

## Step 1: Create the Database & Table (MySQL)

```sql
CREATE DATABASE user_auth_db;

USE user_auth_db;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

👉 **Teaching point:**

* `password` length is large because we’ll store **hashed passwords**, not plain text.

---

## Step 2: Configure MySQL in `application.properties`

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/user_auth_db
spring.datasource.username=root
spring.datasource.password=yourpassword

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
```

👉 **Teaching point:**

* `ddl-auto=update` automatically syncs entity → table.

---

## Step 3: Create User Entity (`User.java`)

```java
package entity;

import jakarta.persistence.*;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String username;
    private String email;
    private String password;

    // getters and setters
}
```

👉 **Teaching point:**

* `@Entity` maps Java class → DB table

---

## Step 4: Create Repository (`UserRepository.java`)

```java
package repository;

import org.springframework.data.jpa.repository.JpaRepository;
import entity.User;

public interface UserRepository extends JpaRepository<User, Integer> {

    boolean existsByUsername(String username);
    boolean existsByEmail(String email);
}
```

👉 **Teaching point:**

* Spring Data JPA **auto-generates queries**

---

## Step 5: Create DTO for Sign-Up Request (Best Practice)

```java
package dto;

public class SignupRequest {
    public String username;
    public String email;
    public String password;
}
```

👉 **Teaching point:**

* DTO prevents exposing entity directly

---

## Step 6: Create Service Layer (`UserService.java`)

```java
package service;

import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;

import entity.User;
import repository.UserRepository;

@Service
public class UserService {

    private final UserRepository userRepository;
    private final BCryptPasswordEncoder encoder = new BCryptPasswordEncoder();

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public String register(User user) {

        if (userRepository.existsByUsername(user.getUsername())) {
            return "Username already exists";
        }

        if (userRepository.existsByEmail(user.getEmail())) {
            return "Email already exists";
        }

        user.setPassword(encoder.encode(user.getPassword()));
        userRepository.save(user);

        return "User registered successfully";
    }
}
```

👉 **Teaching point:**

* **Never store plain passwords**
* BCrypt hashing is industry standard

---

## Step 7: Create Controller (`AuthController.java`)

```java
package controller;

import org.springframework.web.bind.annotation.*;

import dto.SignupRequest;
import entity.User;
import service.UserService;

@RestController
@RequestMapping("/auth")
public class AuthController {

    private final UserService userService;

    public AuthController(UserService userService) {
        this.userService = userService;
    }

    @PostMapping("/signup")
    public String signup(@RequestBody SignupRequest request) {

        User user = new User();
        user.setUsername(request.username);
        user.setEmail(request.email);
        user.setPassword(request.password);

        return userService.register(user);
    }
}
```

👉 **Teaching point:**

* REST API consumes **JSON**
* `@RequestBody` maps JSON → Java

---

## Step 8: Test Using Postman / Curl

**POST URL**

```
http://localhost:8080/auth/signup
```

**JSON Body**

```json
{
  "username": "rahul",
  "email": "rahul@gmail.com",
  "password": "rahul123"
}
```

✔ Response:

```
User registered successfully
```

---

## Step 9: Verify in MySQL

```sql
SELECT * FROM users;
```

👉 You’ll see:

* Password stored as **hashed**
* Auto-generated ID

---

## What You Can Add Later (Next Demos)

* Login API
* JWT Authentication
* Role-based users (ADMIN / USER)
* Email verification
* Validation annotations (`@NotNull`, `@Email`)

---

## Perfect for Teaching Because:

* Clear **Controller → Service → Repository**
* Real-world practices (DTO, hashing)
* Easy to extend into **full authentication**

If you want, I can next give:

* **Login flow**
* **JWT-based authentication**
* **Same demo converted into Microservice style**

Just tell me 👍
