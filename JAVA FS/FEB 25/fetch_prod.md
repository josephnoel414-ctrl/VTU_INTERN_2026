Great demo idea 👍 — this is **perfect for students who already know Spring Boot + MySQL + microservices basics**.

Below is a **clean, classroom-friendly end-to-end demo**:

> **Scenario**
>
> * MySQL has a `products` table (≈ 5 records)
> * Spring Boot fetches product data
> * Data is rendered using **basic HTML + minimal CSS** (no React, no JS framework)

---

## 1️⃣ MySQL: Products Table

### SQL Schema

```sql
CREATE DATABASE product_demo;
USE product_demo;

CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2),
    quantity INT
);
```

### Sample Data (5 Products)

```sql
INSERT INTO products (name, category, price, quantity) VALUES
('Laptop', 'Electronics', 65000, 10),
('Mobile Phone', 'Electronics', 30000, 25),
('Office Chair', 'Furniture', 8500, 15),
('Notebook', 'Stationery', 60, 200),
('Water Bottle', 'Accessories', 450, 50);
```

---

## 2️⃣ Spring Boot Project Setup

### Dependencies (pom.xml)

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
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
</dependencies>
```

---

## 3️⃣ application.properties

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/product_demo
spring.datasource.username=root
spring.datasource.password=your_password

spring.jpa.hibernate.ddl-auto=none
spring.jpa.show-sql=true

spring.thymeleaf.cache=false
```

---

## 4️⃣ Entity Layer – Product

```java
package com.example.productdemo.entity;

import jakarta.persistence.*;

@Entity
@Table(name = "products")
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String name;
    private String category;
    private double price;
    private int quantity;

    // Getters & Setters
}
```

---

## 5️⃣ Repository Layer

```java
package com.example.productdemo.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import com.example.productdemo.entity.Product;

public interface ProductRepository extends JpaRepository<Product, Integer> {
}
```

---

## 6️⃣ Service Layer (Optional but Recommended)

```java
package com.example.productdemo.service;

import java.util.List;
import org.springframework.stereotype.Service;
import com.example.productdemo.entity.Product;
import com.example.productdemo.repository.ProductRepository;

@Service
public class ProductService {

    private final ProductRepository repo;

    public ProductService(ProductRepository repo) {
        this.repo = repo;
    }

    public List<Product> getAllProducts() {
        return repo.findAll();
    }
}
```

---

## 7️⃣ Controller – Fetch & Send Data to HTML

```java
package com.example.productdemo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import com.example.productdemo.service.ProductService;

@Controller
public class ProductController {

    private final ProductService service;

    public ProductController(ProductService service) {
        this.service = service;
    }

    @GetMapping("/products")
    public String showProducts(Model model) {
        model.addAttribute("products", service.getAllProducts());
        return "products";
    }
}
```

---

## 8️⃣ HTML View (Thymeleaf)

📁 `src/main/resources/templates/products.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Product List</title>
    <style>
        body {
            font-family: Arial;
            background-color: #f4f4f4;
            padding: 20px;
        }
        table {
            border-collapse: collapse;
            width: 80%;
            background: white;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 10px;
            text-align: center;
        }
        th {
            background-color: #2c3e50;
            color: white;
        }
    </style>
</head>
<body>

<h2>Available Products</h2>

<table>
    <tr>
        <th>ID</th>
        <th>Name</th>
        <th>Category</th>
        <th>Price (₹)</th>
        <th>Quantity</th>
    </tr>

    <tr th:each="p : ${products}">
        <td th:text="${p.id}"></td>
        <td th:text="${p.name}"></td>
        <td th:text="${p.category}"></td>
        <td th:text="${p.price}"></td>
        <td th:text="${p.quantity}"></td>
    </tr>
</table>

</body>
</html>
```

---

## 9️⃣ Run & Demo Flow (For Students)

1. Start **MySQL**
2. Run Spring Boot application
3. Open browser →
   👉 `http://localhost:8080/products`
4. Explain:

   * DB → JPA → Service → Controller → HTML
   * **No REST API yet**, pure server-side rendering



Just tell me 👍
