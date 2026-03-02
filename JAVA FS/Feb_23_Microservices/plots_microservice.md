Perfect 👍
Since you **already have a running Spring REST application in Eclipse**, we’ll **only add what’s needed** to create the **Plots microservice** and its **plots_db** in MySQL.

I’ll split this into **clear, demo-friendly steps** (ideal for teaching).

---

# PART 1: MySQL – Create `plots_db`

## Step 1: Open MySQL

Use **MySQL Workbench** or command line.

```sql
SHOW DATABASES;
```

---

## Step 2: Create Database

```sql
CREATE DATABASE plot_db;
```

Verify:

```sql
SHOW DATABASES;
```

---

## Step 3: Use the Database

```sql
USE plot_db;
```

---

## Step 4: Create `plots` Table

(Simple and realistic for your Real Estate demo)

```sql
CREATE TABLE plots (
    id INT AUTO_INCREMENT PRIMARY KEY,
    plot_no VARCHAR(20) NOT NULL,
    facing VARCHAR(20),
    size INT,
    price DECIMAL(10,2),
    status VARCHAR(20) DEFAULT 'AVAILABLE'
);
```

---

## Step 5: Insert Sample Data (Optional)

```sql
INSERT INTO plots (plot_no, facing, size, price)
VALUES 
('P101', 'East', 1200, 4500000),
('P102', 'West', 1500, 5500000);
```

Check:

```sql
SELECT * FROM plots;
```

✅ **MySQL side is done**

---

# PART 2: Eclipse – Create / Configure Plot Service

You already have:

* Spring Boot REST app
* Booking service running on **8081**

Now we create **Plot Service (8082)**.

---

## Step 6: Create a New Spring Boot Project (Recommended)

In **Eclipse**:

```
File → New → Spring Starter Project
```

### Project Details

* **Name**: plot-service
* **Type**: Maven
* **Packaging**: Jar
* **Java**: 17 / 11
* **Port**: will set later

### Dependencies

✔ Spring Web
✔ Spring Data JPA
✔ MySQL Driver

Finish ✅

---

## Step 7: Configure `application.properties`

📍 `src/main/resources/application.properties`

```properties
server.port=8082

spring.datasource.url=jdbc:mysql://localhost:3306/plot_db
spring.datasource.username=root
spring.datasource.password=your_password

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
```

💡 **Why `ddl-auto=update`?**

* Table auto-sync during development
* Good for demos

---

# PART 3: Create Plot Microservice Layers

## Step 8: Create Plot Entity

📍 `entity/Plot.java`

```java
package com.example.plotservice.entity;

import jakarta.persistence.*;

@Entity
@Table(name = "plots")
public class Plot {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String plotNo;
    private String facing;
    private int size;
    private double price;
    private String status;

    // getters & setters
}
```

---

## Step 9: Create Repository

📍 `repository/PlotRepository.java`

```java
package com.example.plotservice.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import com.example.plotservice.entity.Plot;

public interface PlotRepository extends JpaRepository<Plot, Integer> {
}
```

---

## Step 10: Create REST Controller

📍 `controller/PlotController.java`

```java
package com.example.plotservice.controller;

import java.util.List;

import org.springframework.web.bind.annotation.*;

import com.example.plotservice.entity.Plot;
import com.example.plotservice.repository.PlotRepository;

@RestController
@RequestMapping("/plots")
public class PlotController {

    private final PlotRepository plotRepository;

    public PlotController(PlotRepository plotRepository) {
        this.plotRepository = plotRepository;
    }

    // GET all plots
    @GetMapping
    public List<Plot> getAllPlots() {
        return plotRepository.findAll();
    }

    // POST create plot
    @PostMapping
    public Plot createPlot(@RequestBody Plot plot) {
        return plotRepository.save(plot);
    }
}
```

---

# PART 4: Run & Verify

## Step 11: Run Plot Service

In Eclipse:

```
Right Click → Run As → Spring Boot App
```

Console should show:

```
Tomcat started on port 8082
```

---

## Step 12: Test Using Postman

### GET all plots

```
GET http://localhost:8082/plots
```

### POST create plot

```
POST http://localhost:8082/plots
```

```json
{
  "plotNo": "P103",
  "facing": "North",
  "size": 1800,
  "price": 6500000,
  "status": "AVAILABLE"
}
```

Check MySQL:

```sql
SELECT * FROM plots;
```

✅ Data saved in **plot_db**

---

# PART 5: How This Fits Your Diagram (Interview-Ready)

✔ Booking Service → **NO direct DB access to plot_db**
✔ Booking Service → **calls Plot Service API**
✔ Each microservice → **owns its database**

👉 This is **true microservice design**

---

## Next logical steps (tell me when ready):

1. Booking Service → call Plot Service using **RestTemplate / WebClient**
2. Reserve a plot during booking
3. Handle **plot status update**
4. Add **PDF upload** for plot documents (your earlier idea)

**properly in draw.io steps**
