Great—this fits perfectly with a **modern MERN microservices** demo 👍
Below is a **clean, industry-aligned Sign-Up module** your students can **follow step-by-step and implement** using **MongoDB + Express + Node**, assuming React UI already exists or will be added later.

I’ll keep it **training-friendly**, but aligned with **real-world best practices**.

---

# MERN Sign-Up Module (Modern Best Practices)

## 1️⃣ Define the Sign-Up Requirements (Before Coding)

**User inputs**

* `username`
* `email`
* `password`

**Modern standards**

* Email must be **unique**
* Password must be **hashed** (never stored in plain text)
* Basic validation (length, format)
* Clear API response messages
* Ready for JWT-based login later

---

## 2️⃣ Microservice Responsibility (Auth Service)

Create (or use) a dedicated **Auth/User microservice**

**Responsibilities**

* Register user (Sign-Up)
* Authenticate user (Login – later)
* Manage password security

**Database**

* MongoDB → `users` collection

---

## 3️⃣ MongoDB User Schema (Best Practice)

📁 `models/User.js`

```js
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema(
  {
    username: {
      type: String,
      required: true,
      trim: true,
      minlength: 3
    },
    email: {
      type: String,
      required: true,
      unique: true,
      lowercase: true
    },
    password: {
      type: String,
      required: true,
      minlength: 6
    }
  },
  { timestamps: true }
);

module.exports = mongoose.model("User", userSchema);
```

✅ Why this matters:

* `unique: true` → prevents duplicate emails
* `timestamps` → industry standard
* No plain-text password storage

---

## 4️⃣ Password Hashing (MANDATORY)

Install dependency:

```bash
npm install bcryptjs
```

📌 Rule to tell students:

> **Passwords are NEVER saved directly — always hash them**

---

## 5️⃣ Sign-Up API Endpoint

📁 `routes/auth.routes.js`

```js
const express = require("express");
const bcrypt = require("bcryptjs");
const User = require("../models/User");

const router = express.Router();

router.post("/signup", async (req, res) => {
  try {
    const { username, email, password } = req.body;

    // 1. Basic validation
    if (!username || !email || !password) {
      return res.status(400).json({ message: "All fields are required" });
    }

    // 2. Check if user already exists
    const existingUser = await User.findOne({ email });
    if (existingUser) {
      return res.status(409).json({ message: "Email already registered" });
    }

    // 3. Hash password
    const salt = await bcrypt.genSalt(10);
    const hashedPassword = await bcrypt.hash(password, salt);

    // 4. Save user
    const user = new User({
      username,
      email,
      password: hashedPassword
    });

    await user.save();

    // 5. Response
    res.status(201).json({
      message: "User registered successfully",
      userId: user._id
    });

  } catch (error) {
    res.status(500).json({ message: "Server error" });
  }
});

module.exports = router;
```

---

## 6️⃣ Connect Route to Express App

📁 `app.js` or `server.js`

```js
const express = require("express");
const mongoose = require("mongoose");
const authRoutes = require("./routes/auth.routes");

const app = express();

app.use(express.json());

mongoose.connect("mongodb://localhost:27017/auth_db")
  .then(() => console.log("MongoDB connected"));

app.use("/api/auth", authRoutes);

app.listen(5000, () => {
  console.log("Auth service running on port 5000");
});
```

---

## 7️⃣ API Contract (Very Important for Microservices)

Tell students to **document this clearly** 👇

### Endpoint

```
POST /api/auth/signup
```

### Request Body

```json
{
  "username": "rahul",
  "email": "rahul@gmail.com",
  "password": "rahul@123"
}
```

### Success Response

```json
{
  "message": "User registered successfully",
  "userId": "65f8a1c9..."
}
```

### Error Scenarios

* 400 → Missing fields
* 409 → Email already exists
* 500 → Server error

---

## 8️⃣ Testing the Sign-Up (Postman / Thunder Client)

**Steps for students**

1. Start MongoDB
2. Start Auth Service
3. Open Postman
4. POST request → `/api/auth/signup`
5. Verify:

   * User saved in DB
   * Password is hashed
   * Duplicate email blocked

---

## 9️⃣ Best Practices to Emphasize to Students

📌 **Security**

* Never return password in response
* Hash passwords
* Use environment variables for DB URL

📌 **Scalability**

* Auth service is independent
* Other microservices will trust Auth service later

📌 **Industry Alignment**

* Same structure used in real products
* JWT, refresh tokens can be layered later

