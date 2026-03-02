Got it 👍
Below is a **clean, step-by-step LAB document** meant **only for MERN students**.
No comparisons, no theory overload — **follow → code → run → verify** style.

You can copy-paste this directly into your LMS / PDF / Google Doc.

---

# 🧪 LAB: Product Listing & Details App using MERN Stack

## 🎯 Objective

Build a MERN application that:

1. Stores product data in MongoDB
2. Fetches and displays products in a table
3. Allows searching by product name or category
4. Opens a product details page when product name is clicked

---

## 🧰 Prerequisites

* Node.js installed
* MongoDB (local or Atlas)
* Basic knowledge of:

  * Express
  * React
  * REST APIs

---

## 📁 Folder Structure

```
product-demo-mern/
│
├── backend/
│   ├── server.js
│   ├── config/
│   │    └── db.js
│   ├── models/
│   │    └── Product.js
│   └── routes/
│        └── productRoutes.js
│
└── frontend/
    └── src/
         ├── App.jsx
         └── pages/
              ├── ProductList.jsx
              └── ProductDetails.jsx
```

---

# 🔹 PART 1: Backend (Node + Express + MongoDB)

## Step 1: Initialize Backend

```bash
mkdir backend
cd backend
npm init -y
npm install express mongoose cors
```

---

## Step 2: MongoDB Connection

### `config/db.js`

```js
const mongoose = require("mongoose");

const connectDB = async () => {
  await mongoose.connect("mongodb://127.0.0.1:27017/product_demo");
  console.log("MongoDB Connected");
};

module.exports = connectDB;
```

---

## Step 3: Product Schema

### `models/Product.js`

```js
const mongoose = require("mongoose");

const productSchema = new mongoose.Schema({
  name: String,
  category: String,
  price: Number,
  quantity: Number
});

module.exports = mongoose.model("Product", productSchema);
```

---

## Step 4: Product Routes

### `routes/productRoutes.js`

```js
const express = require("express");
const Product = require("../models/Product");

const router = express.Router();

/* Fetch all products */
router.get("/", async (req, res) => {
  const products = await Product.find();
  res.json(products);
});

/* Search products */
router.get("/search", async (req, res) => {
  const keyword = req.query.q;

  const products = await Product.find({
    $or: [
      { name: { $regex: keyword, $options: "i" } },
      { category: { $regex: keyword, $options: "i" } }
    ]
  });

  res.json(products);
});

/* Get product by ID */
router.get("/:id", async (req, res) => {
  const product = await Product.findById(req.params.id);
  res.json(product);
});

module.exports = router;
```

---

## Step 5: Express Server

### `server.js`

```js
const express = require("express");
const cors = require("cors");
const connectDB = require("./config/db");
const productRoutes = require("./routes/productRoutes");

const app = express();
connectDB();

app.use(cors());
app.use(express.json());

app.use("/api/products", productRoutes);

app.listen(5000, () => {
  console.log("Server running on port 5000");
});
```

---

## Step 6: Insert Sample Data (MongoDB)

Use **MongoDB Compass** or shell:

```js
db.products.insertMany([
  { name: "Laptop", category: "Electronics", price: 65000, quantity: 10 },
  { name: "Mobile Phone", category: "Electronics", price: 30000, quantity: 25 },
  { name: "Office Chair", category: "Furniture", price: 8500, quantity: 15 },
  { name: "Notebook", category: "Stationery", price: 60, quantity: 200 },
  { name: "Water Bottle", category: "Accessories", price: 450, quantity: 50 }
])
```

---

## ▶️ Run Backend

```bash
node server.js
```

---

# 🔹 PART 2: Frontend (React)

## Step 7: Create React App

```bash
npm create vite@latest frontend
cd frontend
npm install axios react-router-dom
npm run dev
```

---

## Step 8: Routing Setup

### `App.jsx`

```jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
import ProductList from "./pages/ProductList";
import ProductDetails from "./pages/ProductDetails";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<ProductList />} />
        <Route path="/products/:id" element={<ProductDetails />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

---

## Step 9: Product List Page

### `pages/ProductList.jsx`

```jsx
import { useEffect, useState } from "react";
import axios from "axios";
import { Link } from "react-router-dom";

function ProductList() {
  const [products, setProducts] = useState([]);
  const [search, setSearch] = useState("");

  useEffect(() => {
    axios.get("http://localhost:5000/api/products")
      .then(res => setProducts(res.data));
  }, []);

  const handleSearch = () => {
    axios.get(`http://localhost:5000/api/products/search?q=${search}`)
      .then(res => setProducts(res.data));
  };

  return (
    <>
      <h2>Available Products</h2>

      <input
        placeholder="Search name or category"
        onChange={(e) => setSearch(e.target.value)}
      />
      <button onClick={handleSearch}>Search</button>

      <table border="1" cellPadding="8">
        <tr>
          <th>Name</th>
          <th>Category</th>
          <th>Price</th>
          <th>Quantity</th>
        </tr>

        {products.map(p => (
          <tr key={p._id}>
            <td>
              <Link to={`/products/${p._id}`}>{p.name}</Link>
            </td>
            <td>{p.category}</td>
            <td>{p.price}</td>
            <td>{p.quantity}</td>
          </tr>
        ))}
      </table>
    </>
  );
}

export default ProductList;
```

---

## Step 🔟: Product Details Page

### `pages/ProductDetails.jsx`

```jsx
import { useParams } from "react-router-dom";
import { useEffect, useState } from "react";
import axios from "axios";

function ProductDetails() {
  const { id } = useParams();
  const [product, setProduct] = useState({});

  useEffect(() => {
    axios.get(`http://localhost:5000/api/products/${id}`)
      .then(res => setProduct(res.data));
  }, [id]);

  return (
    <>
      <h1>{product.name}</h1>

      <p><strong>🚧 Site Under Construction 🚧</strong></p>
      <p>Product details will be available soon.</p>
    </>
  );
}

export default ProductDetails;
```

---

# ✅ Expected Output

* Product list displayed in table
* Search filters by name or category
* Clicking product name opens a new page
* Product name shown as heading
* Static placeholder message displayed

---

# 📝 LAB Completion Checklist

* [ ] MongoDB connected
* [ ] Products inserted
* [ ] Backend APIs working
* [ ] React table rendering
* [ ] Search working
* [ ] Product details page opens

