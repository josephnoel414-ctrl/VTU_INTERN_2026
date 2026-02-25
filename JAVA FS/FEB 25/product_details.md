Great extension 👍 — this is a **very natural next step** after list + search, and it introduces **navigation + path variables** cleanly.

Below is a **simple, classroom-friendly solution** using **Spring Boot + Thymeleaf**, with **minimal CSS** and **static placeholder content** as you requested.

---

# 🎯 Goal Recap

From the product list page:

* Clicking **Product Name**
* Opens a **new page**
* Shows:

  * Product name as **bold heading**
  * Static text: *“Site under construction”*

---

## 1️⃣ Update the Product List HTML (Clickable Product Name)

### `products.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Available Products</title>
    <style>
        table {
            border-collapse: collapse;
            width: 80%;
        }
        th, td {
            border: 1px solid #ccc;
            padding: 8px;
        }
        th {
            background-color: #2c3e50;
            color: white;
        }
        a {
            text-decoration: none;
            color: #0066cc;
        }
        a:hover {
            text-decoration: underline;
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

    <tr th:each="product : ${products}">
        <td th:text="${product.id}"></td>

        <!-- CLICKABLE PRODUCT NAME -->
        <td>
            <a th:href="@{/products/{id}(id=${product.id})}"
               th:text="${product.name}">
            </a>
        </td>

        <td th:text="${product.category}"></td>
        <td th:text="${product.price}"></td>
        <td th:text="${product.quantity}"></td>
    </tr>
</table>

</body>
</html>
```

👉 **Key teaching point**:

> Clicking name → `/products/{id}`

---

## 2️⃣ Add Controller Method for Product Details Page

### `ProductController.java`

```java
@Controller
@RequestMapping("/products")
public class ProductController {

    @Autowired
    private ProductRepository productRepository;

    // Existing list/search methods stay as is

    @GetMapping("/{id}")
    public String showProductDetails(@PathVariable int id, Model model) {

        Product product = productRepository.findById(id).orElse(null);

        model.addAttribute("product", product);
        return "product-details";
    }
}
```

👉 **What students learn here**:

* `@PathVariable`
* Passing data to a new page
* URL-based navigation

---

## 3️⃣ Create the New Product Details Page

### `product-details.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Product Details</title>
    <style>
        body {
            font-family: Arial;
            margin: 30px;
        }
        h1 {
            color: #2c3e50;
        }
        .box {
            margin-top: 20px;
            padding: 15px;
            border: 1px dashed #999;
            background-color: #f9f9f9;
        }
        a {
            display: inline-block;
            margin-top: 20px;
        }
    </style>
</head>
<body>

<h1 th:text="${product.name}"></h1>

<div class="box">
    <p><strong>🚧 Site Under Construction 🚧</strong></p>
    <p>
        Detailed product information will be available soon.
        Thank you for your patience!
    </p>
</div>

<a href="/products">← Back to Products</a>

</body>
</html>
```

---

## 4️⃣ Final Flow (Explain to Students)

```
Product List Page
   ↓ (click product name)
GET /products/3
   ↓
Controller fetches product
   ↓
Product Details Page
```

---

## 🧠 How to Explain This in One Line

> “Clicking a product name sends its **ID in the URL**,
> the controller fetches it, and a **new page is rendered**.”

---
