Below are **real-estate-themed SQL examples**



---

## 🏠 Assumed Tables

```sql
plots(plot_id, plot_no, length, width, facing, price)
documents(doc_id, plot_id, file_name)
builders(builder_id, name)
```

---

## 1️⃣ List all plots

```sql
SELECT * FROM plots;
```

---

## 2️⃣ Show only East-facing plots

```sql
SELECT * FROM plots
WHERE facing = 'E';
```

---

## 3️⃣ Show plots larger than 1500 sq ft

```sql
SELECT plot_no, (length * width) AS area
FROM plots
WHERE (length * width) > 1500;
```

---

## 4️⃣ Count how many plots exist

```sql
SELECT COUNT(*) FROM plots;
```

---

## 5️⃣ Average plot price

```sql
SELECT AVG(price) FROM plots;
```

---

## 6️⃣ Sort plots by price (low → high)

```sql
SELECT plot_no, price
FROM plots
ORDER BY price ASC;
```

---

## 7️⃣ Find most expensive plot

```sql
SELECT plot_no, price
FROM plots
ORDER BY price DESC
LIMIT 1;
```

---

## 8️⃣ Show plots with uploaded documents

```sql
SELECT DISTINCT p.plot_no
FROM plots p
JOIN documents d ON p.plot_id = d.plot_id;
```

---

## 9️⃣ Show documents for a given plot

```sql
SELECT d.file_name
FROM documents d
JOIN plots p ON d.plot_id = p.plot_id
WHERE p.plot_no = 'P-101';
```

---

## 🔟 Count documents per plot

```sql
SELECT p.plot_no, COUNT(d.doc_id) AS document_count
FROM plots p
LEFT JOIN documents d ON p.plot_id = d.plot_id
GROUP BY p.plot_no;
```

---

## 1️⃣1️⃣ Insert a new plot (Builder upload)

```sql
INSERT INTO plots (plot_no, length, width, facing, price)
VALUES ('P-110', 30, 50, 'N', 4500000);
```

---

## 1️⃣2️⃣ Update plot price

```sql
UPDATE plots
SET price = 4800000
WHERE plot_no = 'P-110';
```

---

## 1️⃣3️⃣ Delete a plot

```sql
DELETE FROM plots
WHERE plot_no = 'P-110';
```

---

## 1️⃣4️⃣ Buyer-style search (Facing + Budget)

```sql
SELECT plot_no, price
FROM plots
WHERE facing = 'E'
AND price < 5000000;
```

---

## 1️⃣5️⃣ Show plots without documents

```sql
SELECT p.plot_no
FROM plots p
LEFT JOIN documents d ON p.plot_id = d.plot_id
WHERE d.doc_id IS NULL;
```


