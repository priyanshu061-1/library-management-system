
# 📚 Library Management System - SQL Project

This project demonstrates the core functionalities of a Library Management System using SQL, with real-world operations such as CRUD tasks, CTAS (Create Table As Select), and analytical queries for branch and member management.

## 🗂️ Database Schema Overview

The system contains the following main tables:
- `books`
- `members`
- `employees`
- `branch`
- `issue_status`
- `return_status`

### 📊 Entity Relationship Diagram

![Library System ER Diagram](library_erd.png)

---

## 🎯 Project Objectives & Tasks

### ✅ **Task 1: CRUD Operations**

**1. Create:**  
Insert a sample book record:
```sql
INSERT INTO books VALUES (
  '978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.'
);
```

**2. Update:**  
Update an existing member’s address:
```sql
UPDATE members
SET member_address = 'New Street 42, Downtown'
WHERE member_id = 'M102';
```

**3. Delete:**  
Delete record from `issued_status`:
```sql
DELETE FROM issue_status
WHERE issued_id = 'IS121';
```

**4. Read:**  
Retrieve all books issued by employee `'E101'`:
```sql
SELECT * FROM issue_status
WHERE issued_emp_id = 'E101';
```

---

### ✅ **Task 2: Advanced Queries**

**List Members Who Have Issued More Than One Book:**
```sql
SELECT issued_member_id, COUNT(*) AS books_issued
FROM issue_status
GROUP BY issued_member_id
HAVING COUNT(*) > 1;
```

---

### ✅ **Task 3: CTAS (Create Table As Select)**

**Create Summary Table of Issued Book Count Per Title:**
```sql
CREATE TABLE book_issue_summary AS
SELECT issued_book_name, COUNT(*) AS book_issued_cnt
FROM issue_status
GROUP BY issued_book_name;
```

---

### ✅ **Task 4: Return & Overdue Tracking**

**Identify Members with Overdue Books (30-day period):**
```sql
SELECT m.member_id, m.member_name, b.book_title, i.issued_date,
       CURRENT_DATE - i.issued_date AS days_overdue
FROM members m
JOIN issue_status i ON m.member_id = i.issued_member_id
JOIN books b ON b.isbn = i.issued_book_isbn
WHERE CURRENT_DATE - i.issued_date > 30;
```

**Update Book Status to 'yes' on Return:**
```sql
UPDATE books
SET status = 'yes'
WHERE isbn IN (
  SELECT return_book_isbn FROM return_status
);
```

---

### ✅ **Task 5: Reports and Analytics**

**Branch Performance Report:**
```sql
SELECT e.branch_id,
       COUNT(DISTINCT i.issued_id) AS books_issued,
       COUNT(DISTINCT r.return_id) AS books_returned,
       SUM(b.rental_price) AS total_revenue
FROM employees e
JOIN issue_status i ON e.emp_id = i.issued_emp_id
JOIN books b ON b.isbn = i.issued_book_isbn
LEFT JOIN return_status r ON i.issued_id = r.issued_id
GROUP BY e.branch_id;
```

---

### ✅ **Task 6: Active Members Table (CTAS)**

**Create Table of Members Issuing Books in Last 2 Months:**
```sql
CREATE TABLE active_members AS
SELECT DISTINCT m.*
FROM members m
JOIN issue_status i ON m.member_id = i.issued_member_id
WHERE i.issued_date >= CURRENT_DATE - INTERVAL '2 months';
```

---

## 🛠️ Technologies Used

- PostgreSQL / MySQL
- ER Diagram Design (dbdiagram.io / pgModeler / DBeaver)
- SQL CRUD and CTAS
- Aggregate & JOIN Queries

---

## 📌 Author

**Priyanshu Dubey**  
📧 Email: priyanshudubey061@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/your-profile/)  
💻 [GitHub](https://github.com/yourusername)

---

## 📸 Screenshot

You can update and include sample query results or a screenshot of your SQL console here.

---

> _Feel free to fork, improve, or suggest new features for this database system!_
