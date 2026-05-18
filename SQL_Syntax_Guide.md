# Comprehensive DBMS & SQL Study Notes

This document serves as a complete set of notes covering Database Management Systems (DBMS) theory, SQL syntax, and practical examples.

---

## 📚 Part 1: Core DBMS Theory

### 1. What is a DBMS?
A **Database Management System (DBMS)** is software that interacts with end-users, applications, and the database itself to capture and analyze the data. It provides facilities to store, retrieve, manage, and update data securely.

*   **RDBMS (Relational DBMS):** A type of DBMS that stores data in a structured format, using rows and columns (tables). Examples: MySQL, PostgreSQL, Oracle SQL.

### 2. ACID Properties
In a database system, a transaction must adhere to the **ACID** properties to ensure data validity despite errors, power failures, or other mishaps:
*   **Atomicity:** "All or nothing." A transaction is treated as a single unit, which either succeeds completely or fails completely.
*   **Consistency:** Data must meet all validation rules. A transaction brings the database from one valid state to another.
*   **Isolation:** Concurrent transactions occur separately without interfering with each other.
*   **Durability:** Once a transaction has been committed, it will remain so, even in the event of a power loss or crash.

### 3. Database Keys
Keys establish relationships between tables and ensure data uniqueness:
*   **Super Key:** A set of one or more attributes that can uniquely identify a row in a table.
*   **Candidate Key:** A minimal Super Key.
*   **Primary Key (PK):** A Candidate Key chosen to uniquely identify each row. It cannot contain `NULL` values.
*   **Foreign Key (FK):** An attribute in one table that refers to the Primary Key in another table, creating a relationship (Referential Integrity).
*   **Unique Key:** Ensures all values in a column are distinct, but unlike a Primary Key, it can accept a `NULL` value.

### 4. ER (Entity-Relationship) Model
The ER model defines the conceptual view of a database.
*   **Entity:** A real-world object (e.g., `Student`, `Book`). Represented by **rectangles**.
*   **Attribute:** A property of an entity (e.g., `Name`, `Roll_No`). Represented by **ovals**.
    *   *Primary Key Attribute:* Underlined.
    *   *Derived Attribute:* Dashed oval (e.g., `Age` derived from `DOB`).
    *   *Multivalued Attribute:* Double oval (e.g., `Phone Numbers`).
*   **Relationship:** Association between entities (e.g., Student `borrows` Book). Represented by **diamonds**.
*   **Cardinality:** Defines the numerical attributes of the relationship (1:1, 1:N, N:1, M:N).

### 5. Normalization
Normalization is the process of organizing data in a database to reduce redundancy and improve data integrity.
*   **1NF (First Normal Form):** Every column must contain atomic (indivisible) values. No repeating groups.
*   **2NF (Second Normal Form):** Must be in 1NF. All non-key attributes must be fully functionally dependent on the Primary Key (no partial dependency).
*   **3NF (Third Normal Form):** Must be in 2NF. There must be no transitive dependency (non-key attributes should not depend on other non-key attributes).

---

## 💻 Part 2: SQL Command Categories

SQL (Structured Query Language) is divided into several sub-languages:

### 1. DDL (Data Definition Language)
Defines or modifies the database structure.
*   `CREATE`: Creates a new database, table, or view.
*   `ALTER`: Modifies the structure of an existing table.
*   `DROP`: Deletes an entire database or table completely.
*   `TRUNCATE`: Deletes all data inside a table, but keeps the table structure.
*   `RENAME`: Renames a table.

### 2. DML (Data Manipulation Language)
Manipulates the actual data inside the tables.
*   `INSERT`: Inserts new records.
*   `UPDATE`: Modifies existing records.
*   `DELETE`: Removes existing records.

### 3. DQL (Data Query Language)
Retrieves data from the database.
*   `SELECT`: Retrieves records.

### 4. DCL (Data Control Language)
Manages permissions and access controls.
*   `GRANT`: Gives user access privileges.
*   `REVOKE`: Takes back access privileges.

### 5. TCL (Transaction Control Language)
Manages transactions (groups of DML operations).
*   `COMMIT`: Saves all the work done permanently.
*   `ROLLBACK`: Restores the database to the last committed state.
*   `SAVEPOINT`: Sets a point within a transaction to which you can roll back.

---

## 📖 Part 3: Comprehensive SQL Syntax Guide

### 1. Database Operations
```sql
CREATE DATABASE IF NOT EXISTS school_db;
USE school_db;
DROP DATABASE school_db;
```

### 2. Table Creation and Constraints (DDL)
Constraints enforce rules on data.
```sql
CREATE TABLE Students (
    student_id INT PRIMARY KEY,             -- Uniquely identifies row, no NULLs
    name VARCHAR(100) NOT NULL,             -- Cannot be left empty
    email VARCHAR(100) UNIQUE,              -- Must be distinct across all rows
    age INT CHECK (age >= 18),              -- Must meet the condition
    enrollment_date DATE DEFAULT CURDATE(), -- Uses current date if not provided
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES Departments(id) -- Links to another table
);
```

### 3. Altering Tables (DDL)
```sql
ALTER TABLE Students ADD phone VARCHAR(15);         -- Add column
ALTER TABLE Students DROP COLUMN phone;             -- Remove column
ALTER TABLE Students MODIFY name VARCHAR(150);      -- Change datatype/size
ALTER TABLE Students CHANGE name full_name VARCHAR(150); -- Rename column
```

### 4. Manipulating Data (DML)
```sql
-- INSERT
INSERT INTO Students (student_id, name, age) VALUES (1, 'Alice', 20);

-- UPDATE (Always use WHERE!)
UPDATE Students SET age = 21 WHERE student_id = 1;

-- DELETE (Always use WHERE!)
DELETE FROM Students WHERE student_id = 1;
```

### 5. Querying Data (DQL - SELECT)
```sql
SELECT * FROM Students;                     -- All columns
SELECT name, age FROM Students;             -- Specific columns

-- Filtering with WHERE
SELECT * FROM Students WHERE age > 20;

-- Pattern Matching with LIKE
SELECT * FROM Students WHERE name LIKE 'A%';   -- Starts with A
SELECT * FROM Students WHERE name LIKE '%A';   -- Ends with A
SELECT * FROM Students WHERE name LIKE '%A%';  -- Contains A
SELECT * FROM Students WHERE name LIKE '_a%';  -- Second letter is a

-- Ranges and Sets
SELECT * FROM Students WHERE age BETWEEN 18 AND 22;
SELECT * FROM Students WHERE department_id IN (1, 2, 3);

-- Sorting
SELECT * FROM Students ORDER BY age DESC, name ASC;

-- Removing Duplicates
SELECT DISTINCT department_id FROM Students;
```

### 6. Aggregate Functions and Grouping
Used to perform calculations on a set of values.
```sql
SELECT COUNT(*) FROM Students;              -- Total rows
SELECT SUM(salary) FROM Employees;          -- Sum of values
SELECT AVG(age) FROM Students;              -- Average value
SELECT MAX(score), MIN(score) FROM Exams;   -- Max and Min values

-- GROUP BY (Groups rows with same values)
SELECT department_id, COUNT(*) AS student_count 
FROM Students 
GROUP BY department_id;

-- HAVING (Filtering grouped records - WHERE does not work with aggregates)
SELECT department_id, COUNT(*) 
FROM Students 
GROUP BY department_id 
HAVING COUNT(*) > 5;
```

### 7. SQL Joins
Combines records from multiple tables.
*   **INNER JOIN:** Returns records that have matching values in both tables.
*   **LEFT JOIN:** Returns all records from the left table, and matched records from the right table.
*   **RIGHT JOIN:** Returns all records from the right table, and matched records from the left table.

```sql
SELECT s.name, d.department_name
FROM Students s
INNER JOIN Departments d ON s.department_id = d.id;
```

### 8. Advanced Objects

#### Views
A virtual table that provides a safe and simplified way to look at data.
```sql
CREATE VIEW IT_Students AS
SELECT name, email FROM Students WHERE department_id = 1;

-- Usage:
SELECT * FROM IT_Students;
```

#### Stored Procedures
A block of reusable SQL code. Can perform DML operations (Insert/Update) and doesn't necessarily return a value.
```sql
DELIMITER //
CREATE PROCEDURE UpdateAge(IN s_id INT, IN new_age INT)
BEGIN
    UPDATE Students SET age = new_age WHERE student_id = s_id;
END //
DELIMITER ;

-- Usage:
CALL UpdateAge(1, 22);
```

#### Functions
Similar to procedures, but **must** return a value. Usually used for computations.
```sql
DELIMITER //
CREATE FUNCTION CalculateTax(salary DECIMAL(10,2)) 
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    RETURN salary * 0.15;
END //
DELIMITER ;

-- Usage:
SELECT name, CalculateTax(salary) FROM Employees;
```

#### Triggers
Automatic actions executed before or after a table event (INSERT, UPDATE, DELETE).
```sql
DELIMITER //
CREATE TRIGGER BeforeStudentDelete
BEFORE DELETE ON Students
FOR EACH ROW
BEGIN
    -- OLD refers to the row about to be deleted
    INSERT INTO Deleted_Students_Log (student_id, name, deleted_at) 
    VALUES (OLD.student_id, OLD.name, NOW());
END //
DELIMITER ;
```
