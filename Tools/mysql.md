# 🐬 MySQL 

## 🔧 Connection & Basics
mysql -u username -p                   # Connect to MySQL
mysql -h hostname -u username -p       # Connect to remote MySQL server
exit;                                  # Exit MySQL

## 📂 Database Operations
SHOW DATABASES;                        # List all databases
CREATE DATABASE db_name;               # Create a new database
USE db_name;                           # Select a database
DROP DATABASE db_name;                 # Delete a database

## 🧱 Table Management
SHOW TABLES;                           # Show all tables
DESCRIBE table_name;                   # Show table structure
CREATE TABLE table_name (              # Create a table
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  age INT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
ALTER TABLE table_name ADD column_name VARCHAR(50);  # Add a column
ALTER TABLE table_name DROP COLUMN column_name;       # Remove a column
RENAME TABLE old_name TO new_name;                    # Rename a table
DROP TABLE table_name;                               # Delete a table

## ✏️ CRUD Operations
INSERT INTO table_name (name, age) VALUES ('John', 30);              # Insert data
SELECT * FROM table_name;                                            # Select all rows
SELECT name, age FROM table_name WHERE age > 25;                     # Conditional select
UPDATE table_name SET age = 31 WHERE name = 'John';                  # Update data
DELETE FROM table_name WHERE name = 'John';                          # Delete data

## 🔍 Filtering & Sorting
SELECT * FROM table_name ORDER BY age DESC;                          # Sort results
SELECT * FROM table_name LIMIT 5;                                    # Limit rows
SELECT DISTINCT column_name FROM table_name;                         # Unique values
SELECT * FROM table_name WHERE name LIKE 'J%';                       # Pattern match
SELECT * FROM table_name WHERE age BETWEEN 20 AND 40;                # Range condition

## 🔗 Joins
SELECT a.name, b.salary
FROM employees a
JOIN salaries b ON a.id = b.emp_id;                                  # Inner Join

SELECT a.name, b.salary
FROM employees a
LEFT JOIN salaries b ON a.id = b.emp_id;                             # Left Join

## 🧮 Aggregate Functions
SELECT COUNT(*) FROM table_name;                                     # Count rows
SELECT AVG(age) FROM table_name;                                     # Average value
SELECT SUM(amount) FROM table_name;                                  # Sum
SELECT MIN(age), MAX(age) FROM table_name;                           # Min & Max
SELECT column, COUNT(*) FROM table_name GROUP BY column;             # Group results

## 🔒 User Management
CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';             # Create user
GRANT ALL PRIVILEGES ON db_name.* TO 'user'@'localhost';             # Grant privileges
FLUSH PRIVILEGES;                                                    # Reload privileges
DROP USER 'user'@'localhost';                                        # Delete user

## 🧰 Miscellaneous
SHOW PROCESSLIST;                                                    # Show running queries
SHOW VARIABLES LIKE 'version%';                                      # MySQL version info
SOURCE /path/to/file.sql;                                            # Run SQL script from file
