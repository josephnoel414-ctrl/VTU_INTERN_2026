Great, this fits perfectly as the **next logical demo after Sign-Up** 👍
Below is a **clear, classroom-friendly, step-by-step flow** to implement **Login with existing credentials** in a **MERN setup**, continuing from your **Sign Up microservice**.

I’ll assume:

* **MongoDB DB name**: `user_auth_db`
* Passwords are already **bcrypt-hashed** (as seen in your screenshot)
* Backend is **Node + Express**
* Frontend is **React**

---

## 1️⃣ Understand the Login Flow (Explain First to Students)

**What happens during Login?**

1. User enters **email + password**
2. Backend:

   * Finds user by email in `user_auth_db`
   * Compares entered password with **hashed password** in DB
3. If valid:

   * Generate **JWT token**
   * Send token to frontend
4. React stores token (memory / localStorage)
5. User is considered **logged in**

> 🔑 Key teaching point:
> **Password is NEVER decrypted**. We only compare hashes.

---

## 2️⃣ Backend – Add Login API (Auth Microservice)

### 2.1 Install Required Packages (if not already)

```bash
npm install bcryptjs jsonwebtoken
```

---

### 2.2 Verify MongoDB Connection

```js
mongoose.connect("mongodb://localhost:27017/user_auth_db");
```

> Emphasize: **Same DB, same users collection**

---

### 2.3 User Model (Already Exists – Recap)

```js
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema(
  {
    username: String,
    email: { type: String, unique: true },
    password: String
  },
  { timestamps: true }
);

module.exports = mongoose.model("User", userSchema);
```

---

### 2.4 Create Login Controller

📁 `controllers/authController.js`

```js
const User = require("../models/User");
const bcrypt = require("bcryptjs");
const jwt = require("jsonwebtoken");

exports.login = async (req, res) => {
  const { email, password } = req.body;

  try {
    // 1. Find user by email
    const user = await User.findOne({ email });

    if (!user) {
      return res.status(400).json({ message: "Invalid credentials" });
    }

    // 2. Compare password
    const isMatch = await bcrypt.compare(password, user.password);

    if (!isMatch) {
      return res.status(400).json({ message: "Invalid credentials" });
    }

    // 3. Generate JWT
    const token = jwt.sign(
      { userId: user._id },
      "SECRET_KEY",
      { expiresIn: "1h" }
    );

    // 4. Send response
    res.json({
      message: "Login successful",
      token,
      user: {
        id: user._id,
        username: user.username,
        email: user.email
      }
    });

  } catch (err) {
    res.status(500).json({ message: "Server error" });
  }
};
```

---

### 2.5 Create Login Route

📁 `routes/authRoutes.js`

```js
const express = require("express");
const router = express.Router();
const authController = require("../controllers/authController");

router.post("/login", authController.login);

module.exports = router;
```

---

### 2.6 Register Route in App.js

```js
app.use("/api/auth", require("./routes/authRoutes"));
```

---

### 2.7 Test Login API (Very Important Demo Step)

Use **Postman** or **REST Client**

**POST**

```
http://localhost:5000/api/auth/login
```

**Body (JSON):**

```json
{
  "email": "rahul@gmail.com",
  "password": "rahul123"
}
```

✅ Expected Response:

```json
{
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "...",
    "username": "rahul",
    "email": "rahul@gmail.com"
  }
}
```

> 🎓 Teaching moment:
> Show how **hashed password in DB** still allows login.

---

## 3️⃣ Frontend – React Login UI

### 3.1 Create Login Component

📁 `Login.js`

```jsx
import { useState } from "react";
import axios from "axios";

function Login() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleLogin = async (e) => {
    e.preventDefault();

    try {
      const res = await axios.post(
        "http://localhost:5000/api/auth/login",
        { email, password }
      );

      localStorage.setItem("token", res.data.token);
      alert("Login successful");

    } catch (err) {
      alert("Invalid credentials");
    }
  };

  return (
    <form onSubmit={handleLogin}>
      <h2>Login</h2>

      <input
        type="email"
        placeholder="Email"
        onChange={(e) => setEmail(e.target.value)}
      />

      <input
        type="password"
        placeholder="Password"
        onChange={(e) => setPassword(e.target.value)}
      />

      <button type="submit">Login</button>
    </form>
  );
}

export default Login;
```

---

### 3.2 Add Route in React App

```jsx
<Route path="/login" element={<Login />} />
```

---

## 4️⃣ Explain Token Storage (Conceptual)

Explain to students:

* Token proves **user identity**
* Stored in:

  * `localStorage` (demo-friendly)
  * or memory / cookies (advanced)

Later topics:

* Protected Routes
* Logout
* Token expiry

---

## 5️⃣ Quick Blackboard / Slide Summary

**Login Checklist**

* ✔ Email exists?
* ✔ Password matches hash?
* ✔ JWT generated?
* ✔ Token sent to UI?

---
