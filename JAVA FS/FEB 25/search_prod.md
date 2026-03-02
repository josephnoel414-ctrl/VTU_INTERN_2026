Perfect 👍 — we’ll add **search** in the **simplest, Spring-Boot-friendly way**, without JavaScript frameworks.

We’ll support **search by product name OR category**.

---

# 🔍 Product Search – End-to-End (Spring Boot + MySQL + HTML)

## What Students Will Learn

* How search works **from UI → Controller → Repository → DB**
* Writing **derived queries** in Spring Data JPA
* Using **HTML forms** with Spring Boot

---

## 1️⃣ Repository – Add Search Query

Update `ProductRepository`:

```java
package com.example.productdemo.repository;

import java.util.List;
import org.springframework.data.jpa.repository.JpaRepository;
import com.example.productdemo.entity.Product;

public interface ProductRepository extends JpaRepository<Product, Integer> {

    List<Product> findByNameContainingIgnoreCaseOrCategoryContainingIgnoreCase(
            String name, String category);
}
```

📌 **Explain to students**

* `Containing` → SQL `LIKE %text%`
* `IgnoreCase` → case-insensitive search
* No SQL written → Spring generates query automatically

---

## 2️⃣ Service Layer – Add Search Method

Update `ProductService`:

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

    public List<Product> searchProducts(String keyword) {
        return repo.findByNameContainingIgnoreCaseOrCategoryContainingIgnoreCase(
                keyword, keyword);
    }
}
```

---

## 3️⃣ Controller – Handle Search Request

Update `ProductController`:

```java
package com.example.productdemo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.example.productdemo.service.ProductService;

@Controller
public class ProductController {

    private final ProductService service;

    public ProductController(ProductService service) {
        this.service = service;
    }

    @GetMapping("/products")
    public String showProducts(
            @RequestParam(value = "keyword", required = false) String keyword,
            Model model) {

        if (keyword != null && !keyword.isEmpty()) {
            model.addAttribute("products", service.searchProducts(keyword));
        } else {
            model.addAttribute("products", service.getAllProducts());
        }

        model.addAttribute("keyword", keyword);
        return "products";
    }
}
```

📌 **Teaching moment**

* Same URL `/products`
* Optional query param `?keyword=`
* Clean & REST-friendly

---

## 4️⃣ HTML – Add Search Box

Update `products.html`:

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
            margin-top: 15px;
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
        .search-box {
            margin-bottom: 15px;
        }
        input[type="text"] {
            padding: 8px;
            width: 250px;
        }
        button {
            padding: 8px 15px;
            cursor: pointer;
        }
    </style>
</head>
<body>

<h2>Available Products</h2>

<div class="search-box">
    <form action="/products" method="get">
        <input type="text" name="keyword"
               placeholder="Search by name or category"
               th:value="${keyword}">
        <button type="submit">Search</button>
        <a href="/products">Reset</a>
    </form>
</div>

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

## 5️⃣ How to Demo in Class 🎓

### URLs to Try

```
http://localhost:8080/products
http://localhost:8080/products?keyword=Electronics
http://localhost:8080/products?keyword=laptop
```

### Sample Student Questions to Ask

* ❓ Where does the keyword go from UI?
* ❓ Who talks to DB?
* ❓ Which layer should NOT contain SQL?
* ❓ How would this change in a REST API?

---

## 6️⃣ Clean Architecture Recap

```
HTML (Form)
   ↓
Controller (@RequestParam)
   ↓
Service (business logic)
   ↓
Repository (JPA Query)
   ↓
MySQL
```

