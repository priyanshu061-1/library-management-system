📚 Library Management System - SQL Project
Welcome to the Library Management System project! This project demonstrates comprehensive SQL operations, including CRUD operations, CTAS (Create Table As Select), advanced data querying, and reporting. The system simulates the core functionalities required for managing a library's books, members, employees, and transactions.

🖼️ Project Overview Diagram

![Schema](D:\Library-System-Management---P2\library_erd.png)


📌 Objectives
Practice and demonstrate SQL CRUD operations (Create, Read, Update, Delete).

Create summary reports and track overdue books using advanced SQL.

Use CTAS for generating derived tables from existing data.

Analyze performance across different library branches.

✅ Tasks & Features
🛠️ Task 1: CRUD Operations
Create: Inserted sample records into the books table.

sql
Copy
Edit
INSERT INTO books VALUES ('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');
Read: Retrieved and displayed data from various tables.

Update: Modified records in the employees or members tables.

sql
Copy
Edit
UPDATE members SET address = 'New Delhi, India' WHERE member_id = 'M102';
Delete: Removed entries from the members or issued_status table.

sql
Copy
Edit
DELETE FROM issued_status WHERE issued_id = 'IS121';
📚 Task 2: Advanced Queries
Retrieve All Books Issued by an Employee

sql
Copy
Edit
SELECT * FROM issued_status WHERE emp_id = 'E101';
List Members Who Have Issued More Than One Book

sql
Copy
Edit
SELECT member_id, COUNT(*) AS books_issued 
FROM issued_status 
GROUP BY member_id 
HAVING COUNT(*) > 1;
📊 Task 3: CTAS & Summary Tables
Create Summary Table for Book Issue Counts

sql
Copy
Edit
CREATE TABLE book_summary AS
SELECT book_id, COUNT(*) AS total_book_issued_cnt
FROM issued_status
GROUP BY book_id;
Identify Members with Overdue Books
(Assuming 30-day return policy)

sql
Copy
Edit
SELECT m.member_id, m.name, b.title, i.issue_date, 
       DATEDIFF(CURRENT_DATE, i.issue_date) AS days_overdue
FROM members m
JOIN issued_status i ON m.member_id = i.member_id
JOIN books b ON i.book_id = b.book_id
WHERE DATEDIFF(CURRENT_DATE, i.issue_date) > 30;
Update Book Status Upon Return

sql
Copy
Edit
UPDATE books
SET status = 'Yes'
WHERE book_id IN (SELECT book_id FROM return_status);
Branch Performance Report

sql
Copy
Edit
SELECT branch_id,
       COUNT(DISTINCT issued_status.book_id) AS books_issued,
       COUNT(DISTINCT return_status.book_id) AS books_returned,
       SUM(issued_status.rental_fee) AS total_revenue
FROM issued_status
LEFT JOIN return_status ON issued_status.book_id = return_status.book_id
GROUP BY branch_id;
Create Table of Active Members (Last 2 Months)

sql
Copy
Edit
CREATE TABLE active_members AS
SELECT DISTINCT member_id
FROM issued_status
WHERE issue_date >= DATE_SUB(CURRENT_DATE, INTERVAL 2 MONTH);
💾 Tech Stack
SQL: MySQL / PostgreSQL

Tooling: MySQL Workbench / pgAdmin

Platform: Localhost or Cloud SQL


Priyanshu Dubey
📧 priyanshudubey061@gmail.com
📍 Vidisha, India
🔗 LinkedIn

