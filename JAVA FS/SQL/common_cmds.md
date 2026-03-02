Here is a **clean, classroom-ready cheat list of MySQL commands** commonly used in **MySQL Workbench**.



---

### 🔹 Database-level

```sql
SHOW DATABASES;
```

```sql
USE your_database_name;
```

```sql
SELECT DATABASE();
```

---

### 🔹 Table-level

```sql
SHOW TABLES;
```

```sql
SHOW TABLES LIKE '%plot%';
```

```sql
DESCRIBE table_name;
```

```sql
SHOW COLUMNS FROM table_name;
```

---

### 🔹 Data inspection

```sql
SELECT * FROM table_name;
```

```sql
SELECT * FROM table_name LIMIT 5;
```

```sql
SELECT COUNT(*) FROM table_name;
```

---

### 🔹 Structure & metadata

```sql
SHOW CREATE TABLE table_name;
```

```sql
SHOW INDEX FROM table_name;
```

---

### 🔹 Insert / Update / Delete

```sql
INSERT INTO table_name VALUES (...);
```

```sql
UPDATE table_name SET column=value WHERE condition;
```

```sql
DELETE FROM table_name WHERE condition;
```

---

### 🔹 Filtering & sorting

```sql
SELECT * FROM table_name WHERE column = value;
```

```sql
SELECT * FROM table_name ORDER BY column ASC;
```

```sql
SELECT * FROM table_name ORDER BY column DESC;
```

---

### 🔹 Aggregates

```sql
SELECT COUNT(*) FROM table_name;
```

```sql
SELECT AVG(column) FROM table_name;
```

```sql
SELECT MIN(column), MAX(column) FROM table_name;
```

---

### 🔹 Schema safety

```sql
DROP TABLE table_name;
```

```sql
DROP DATABASE database_name;
```

---

### 🔹 Useful system info

```sql
SHOW PROCESSLIST;
```

```sql
SELECT VERSION();
```

