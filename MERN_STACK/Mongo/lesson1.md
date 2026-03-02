
### 1️⃣ Create a Database

1. Open **MongoDB Compass**
2. Click **“Create Database”**
3. Enter:

   * **Database Name**: `schoolDB`
   * **Collection Name**: `students`
4. Click **Create**

👉 MongoDB always needs **at least one collection** to create a database.

---

### 2️⃣ View Database & Collection

* Left panel → you’ll see:

  * `schoolDB`

    * `students`

Click **students** → you are inside the collection.

---

### 3️⃣ Insert a Document (Single Record)

1. Click **“Add Data”**
2. Choose **“Insert Document”**
3. Replace content with:

```json
{
  "name": "Rahul",
  "age": 21,
  "course": "Computer Science",
  "city": "Bengaluru"
}
```

4. Click **Insert**

✅ This is one MongoDB document (similar to one row in SQL).

---

### 4️⃣ Insert Multiple Documents

Click **Add Data → Insert Document** again and add:

```json
{
  "name": "Anita",
  "age": 22,
  "course": "Information Technology",
  "city": "Pune"
}
```

Now you have **2 documents** in the collection.

---

### 5️⃣ Find / Read Data

* Stay in **Documents** tab
* You’ll automatically see all documents

Try a filter (top bar):

```json
{ "city": "Pune" }
```

👉 Shows only students from Pune.

---

### 6️⃣ Update a Document

1. Hover over a document
2. Click ✏️ **Edit**
3. Change:

   ```json
   "age": 23
   ```
4. Click **Update**

👉 MongoDB updates **only that document**

---

### 7️⃣ Delete a Document

1. Hover over a document
2. Click 🗑 **Delete**
3. Confirm

👉 Only that document is removed (not the collection).

---

### 8️⃣ Create Another Collection

1. Click **schoolDB**
2. Click **“Create Collection”**
3. Collection name: `courses`
4. Click **Create**

Insert a sample document:

```json
{
  "courseName": "Java Full Stack",
  "durationMonths": 6,
  "mode": "Online"
}
```

---

### 9️⃣ Explain MongoDB Concepts (1-Minute Summary)

| SQL         | MongoDB                |
| ----------- | ---------------------- |
| Database    | Database               |
| Table       | Collection             |
| Row         | Document               |
| Column      | Field                  |
| Primary Key | `_id` (auto-generated) |

---

### 🔟 When to Mention MongoDB

Say this clearly to students:

> “MongoDB is a **NoSQL document database**.
> Data is stored as **JSON-like documents**, not tables.”

---

## 🎯 Perfect Teaching Use Case (Your Line)

> “For apps like **student portals, real-estate platforms, or learning systems**, MongoDB lets us store flexible data without strict table rules.”

