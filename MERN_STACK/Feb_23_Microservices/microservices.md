Perfect 👍
Below is a **pure MERN microservices version** of **your exact diagram**, written as **step-by-step implementation + minimal, teachable code**.
This is **ideal for demo, teaching, and interviews**.

---

# MERN Microservices – Booking & Plot (MongoDB)

## Architecture (Same as Your Diagram)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AO9gSXTtX95gjDapTylcjdg.png)

![Image](https://user-images.githubusercontent.com/26835951/112730281-c0d3d680-8f56-11eb-9cd3-07d1965db708.png)

![Image](https://systemdesignschool.io/fundamentals/a-database-per-service/database-per-service-architecture.svg)

```
Client / UI / Postman
        |
        v
Booking Service (8081)  ----->  Plot Service (8082)
        |                            |
     MongoDB                     MongoDB
   booking_db                  plot_db
   bookings                    plots
```

---

## STEP 1️⃣ Plot Service (8082)

### 1.1 Create project

```bash
mkdir plot-service
cd plot-service
npm init -y
npm install express mongoose cors
```

---

### 1.2 `server.js`

```js
const express = require("express");
const mongoose = require("mongoose");
const cors = require("cors");

const app = express();
app.use(cors());
app.use(express.json());

mongoose.connect("mongodb://localhost:27017/plot_db");

app.listen(8082, () => {
  console.log("Plot Service running on port 8082");
});
```

---

### 1.3 Plot Schema (`models/Plot.js`)

```js
const mongoose = require("mongoose");

const plotSchema = new mongoose.Schema({
  plotNo: String,
  facing: String,
  available: Boolean
});

module.exports = mongoose.model("Plot", plotSchema);
```

---

### 1.4 Plot Routes (`routes/plotRoutes.js`)

```js
const express = require("express");
const Plot = require("../models/Plot");

const router = express.Router();

// Add plot
router.post("/plots", async (req, res) => {
  const plot = new Plot(req.body);
  await plot.save();
  res.send(plot);
});

// Get plot by ID
router.get("/plots/:id", async (req, res) => {
  const plot = await Plot.findById(req.params.id);
  res.send(plot);
});

module.exports = router;
```

---

### 1.5 Register routes in `server.js`

```js
const plotRoutes = require("./routes/plotRoutes");
app.use("/", plotRoutes);
```

---

## STEP 2️⃣ Booking Service (8081)

### 2.1 Create project

```bash
mkdir booking-service
cd booking-service
npm init -y
npm install express mongoose axios cors
```

---

### 2.2 `server.js`

```js
const express = require("express");
const mongoose = require("mongoose");
const axios = require("axios");
const cors = require("cors");

const app = express();
app.use(cors());
app.use(express.json());

mongoose.connect("mongodb://localhost:27017/booking_db");

app.listen(8081, () => {
  console.log("Booking Service running on port 8081");
});
```

---

### 2.3 Booking Schema (`models/Booking.js`)

```js
const mongoose = require("mongoose");

const bookingSchema = new mongoose.Schema({
  customerName: String,
  plotId: String,
  status: String
});

module.exports = mongoose.model("Booking", bookingSchema);
```

---

### 2.4 Booking Routes (`routes/bookingRoutes.js`)

```js
const express = require("express");
const Booking = require("../models/Booking");
const axios = require("axios");

const router = express.Router();

// Create booking
router.post("/bookings", async (req, res) => {
  const { customerName, plotId } = req.body;

  // Call Plot Service
  const plotResponse = await axios.get(
    `http://localhost:8082/plots/${plotId}`
  );

  if (!plotResponse.data.available) {
    return res.status(400).send("Plot not available");
  }

  const booking = new Booking({
    customerName,
    plotId,
    status: "CONFIRMED"
  });

  await booking.save();
  res.send(booking);
});

module.exports = router;
```

---

### 2.5 Register routes in `server.js`

```js
const bookingRoutes = require("./routes/bookingRoutes");
app.use("/", bookingRoutes);
```

---

## STEP 3️⃣ MongoDB Structure (What Students Should See)

```text
MongoDB
├── plot_db
│   └── plots
│
└── booking_db
    └── bookings
```

---

## STEP 4️⃣ Testing with Postman

### Add Plot

```
POST http://localhost:8082/plots
{
  "plotNo": "P101",
  "facing": "East",
  "available": true
}
```

### Book Plot

```
POST http://localhost:8081/bookings
{
  "customerName": "Ravi",
  "plotId": "<paste_plot_id_here>"
}
```

---

## Microservices Rules (Clearly Demonstrated)

✔ Database per service
✔ No shared MongoDB collections
✔ Service-to-service REST calls
✔ Independent ports & deployment

---

## Interview-Ready One-Liner

> “In our MERN microservices project, each service owns its MongoDB database and communicates with other services only via REST APIs.”

---



<img width="469" height="630" alt="image" src="https://github.com/user-attachments/assets/0e8c56ab-c73f-4a0b-88b8-cfceb4411467" />


