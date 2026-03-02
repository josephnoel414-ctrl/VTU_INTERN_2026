

## Objective

Run a **bare-bones Maven Spring Boot application** and verify that the server starts successfully.

---

## Step 1: Create a Spring Boot project

Use **Spring Initializr**.

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AKAQfFEwRUq1k7HYcRqG4jg.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AwhnsxO_k-FUGomExKsdUGw.png)

![Image](https://javacodehouse.com/assets/img/spring-boot/Spring%20initializr/Spring%20Initializr-Dependencies-3.jpg)

**Project configuration**

| Field        | Value      |
| ------------ | ---------- |
| Project      | Maven      |
| Language     | Java       |
| Packaging    | Jar        |
| Java         | 17 (or 11) |
| Dependencies | Spring Web |

Generate the project and extract it.

---

## Step 2: Open the project in IDE

Open the extracted folder as an **Existing Maven Project**.

Allow Maven to download all dependencies.

---

## Step 3: Project structure

```
project-root
 ├── src/main/java
 │    └── com/demo/demo
 │         └── DemoApplication.java
 ├── src/main/resources
 │    └── application.properties
 └── pom.xml
```

---

## Step 4: Application entry point

```java
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

---

## Step 5: Run the application

### Option A: From IDE

Run `DemoApplication` as a Java application.

### Option B: Using Maven

```bash
mvn spring-boot:run
```

---

## Step 6: Verify startup

Console output should contain lines similar to:

```
Tomcat started on port(s): 8080
Started DemoApplication
```

---

## Step 7: Browser check

Open the URL:

```
http://localhost:8080
```

The default Whitelabel error page indicates that the server is running and no request mappings are defined.

---

## Step 8: Optional port change

If port 8080 is unavailable, update `application.properties`:

```properties
server.port=9090
```

---

## Result

* Maven build completes
* Spring Boot application starts
* Embedded Tomcat server runs successfully

