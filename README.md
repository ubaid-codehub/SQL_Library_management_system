# SQL_Library_management_system
## Project Overview

This project demonstrates the implementation of a Library Management System using SQL. It includes creating and managing tables, performing CRUD operations, and executing advanced SQL queries. The goal is to showcase skills in database design, manipulation, and querying.

## Objectives
1) Set up the Library Management System Database: Create and populate the database with tables for branches, employees, members, books, issued status, and return status.
2) CRUD Operations: Perform Create, Read, Update, and Delete operations on the data.
3) CTAS (Create Table As Select): Utilize CTAS to create new tables based on query results.
4) Advanced SQL Queries: Develop complex queries to analyze and retrieve specific data.

## Project Structure

- Database Creation: Created a database named library_management_systme.

- Table Creation: Created tables for branches,  employees, members, books, issued status, and return status. Each table includes relevant columns and relationships.








## Usage/Examples

```javascript
REATE TABLE branch (
    branch_id VARCHAR(100) PRIMARY KEY,
    manager_id VARCHAR(100),
    branch_address VARCHAR(100),
    contact_no VARCHAR(100)
);

CREATE TABLE employees(
emp_id VARCHAR(100) PRIMARY KEY,
emp_name VARCHAR(100),
position VARCHAR(100),	
salary INT,	
branch_id VARCHAR(100),
  FOREIGN KEY (branch_id) REFERENCES branch(branch_id)

);

CREATE TABLE members(
member_id VARCHAR(100) PRIMARY KEY,
member_name VARCHAR(100),
member_address VARCHAR(100),
reg_date DATE

);


CREATE TABLE books(
isbn VARCHAR(100) PRIMARY KEY,
book_title VARCHAR(100),
category VARCHAR(100),	
rental_price FLOAT,
status VARCHAR(100),
author VARCHAR(100),
publisher VARCHAR(100)

);

CREATE TABLE issued_status(
issued_id VARCHAR(100) PRIMARY KEY,
issued_member_id VARCHAR(100),
FOREIGN KEY(issued_member_id) REFERENCES members(member_id),
issued_book_name VARCHAR(100),
issued_date DATE,
issued_book_isbn VARCHAR(100),
FOREIGN KEY (issued_book_isbn) REFERENCES books(isbn),
issued_emp_id VARCHAR(100),
FOREIGN KEY (issued_emp_id) REFERENCES employees(emp_id)

);

CREATE TABLE return_status(
return_id VARCHAR(100) PRIMARY KEY,
issued_id VARCHAR(100),
return_book_name VARCHAR(100),
return_date DATE,
return_book_isbn VARCHAR(100),
FOREIGN KEY(return_book_isbn) REFERENCES books(isbn)

); 

```

## CRUD Operations
1) Create: Inserted sample records into the books table.

2) Read: Retrieved and displayed data from various tables.

3) Update: Updated records in the employees table.

4) Delete: Removed records from the members table as needed.

Task 1. Create a New Book Record -- "978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.')"

```javascript
INSERT INTO books(isbn, book_title, category, rental_price, status, author, publisher)
VALUES('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');
SELECT * FROM books;

```

Task 2: Update an Existing Member's Address

```javascript
UPDATE employees
SET SALARY = 70000
WHERE emp_id = 'E107'
select salary from employees where emp_id='E107'

```

Task 3: Delete a Record from the Issued Status Table -- Objective: Delete the record with issued_id = 'IS121' from the issued_status table.

```javascript

DELETE FROM members
WHERE member_id='C119'

```

Task 4: Retrieve All Books Issued by a Specific Employee -- Objective: Select all books issued by the employee with emp_id = 'E101'.


```javascript
SELECT *FROM issued_status 
WHERE issued_emp_id = 'E101'

```

Task 5: List Members Who Have Issued More Than One Book


```javascript
SELECT issued_emp_id, COUNT(*)
FROM issued_status
GROUP BY 1
HAVING COUNT(*) > 1

```

## 3. CTAS (Create Table As Select)
Task 6: Create Summary Tables:Used CTAS to generate new tables based on query results - each book and total book_issued_cnt**

```javascript

CREATE TABLE book_issued_cnt AS
SELECT b.isbn, b.book_title, COUNT(ist.issued_id) AS issue_count
FROM issued_status as ist
JOIN books as b
ON ist.issued_book_isbn = b.isbn
GROUP BY b.isbn, b.book_title;
```

## 4. Data Analysis & Findings

The following SQL queries were used to address specific questions:

Task 7. Retrieve All Books in a Specific Category:

```javascript
SELECT *FROM books
WHERE category = 'Mystery'
```

Task 8: Find Total Rental Income by Category:

```javascript

SELECT 
    b.category,
    SUM(b.rental_price),
    COUNT(*)
FROM 
issued_status as ist
JOIN
books as b
ON b.isbn = ist.issued_book_isbn
GROUP BY 1

```
Task 9: List Members Who Registered in the Last 180 Days:

``` javascript
SELECT *FROM members
WHERE reg_date>= CURRENT_DATE - INTERVAL '180 DAYS'
```

Task 10: List Employees with Their Branch Manager's Name and their branch details:
```javascript

SELECT 
    e1.emp_id, 
    e1.emp_name, 
    e1.position, 
    e1.salary,
    b.*, 
    e2.emp_name AS manager
FROM employees AS e1
JOIN branch AS b ON e1.branch_id = b.branch_id
JOIN employees AS e2 ON b.manager_id = e2.emp_id;
```

Task 11. Create a Table of Books with Rental Price Above a Certain Threshold:

```javascript
CREATE TABLE rental_price AS
SELECT * FROM books 
WHERE rental_price > 20

```

Task 12: Retrieve the List of Books Not Yet Returned

```javascript
SELECT *FROM issued_status isb
JOIN return_status AS rs ON isb.issued_book_isbn = rs.return_book_isbn


```


## Reports
Database Schema: Detailed table structures and relationships.

Data Analysis: Insights into book categories, employee salaries, member registration trends, and issued books.

Summary Reports: Aggregated data on high-demand books and employee performance.

## Conclusion
This project demonstrates the application of SQL skills in creating and managing a library management system. It includes database setup, data manipulation, and advanced querying, providing a solid foundation for data management and analysis.


## Author Ubaid Ul Hassan

- linkedin (linkedin.com/in/ubaid-ul-hassan-773047283)
