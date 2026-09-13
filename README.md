# 📚 Online Bookstore SQL Project

A PostgreSQL-based **Online Bookstore Database Project** designed to practice and demonstrate SQL concepts such as filtering, aggregation, joins, grouping, sorting, subqueries, and analytical queries.

The project contains sample data for books, customers, and orders, along with a collection of basic and advanced SQL queries to analyze the bookstore's data.

---

## 🚀 Project Overview

This project simulates an **online bookstore database** where:

* 📖 Books contain information about titles, authors, genres, prices, and stock.
* 👥 Customers contain customer contact and location information.
* 🛒 Orders contain purchase information, including the customer, book, quantity, date, and total amount.

The project demonstrates how SQL can be used to answer real-world business questions such as:

* Which books belong to a particular genre?
* What are the most expensive books?
* How much revenue has the bookstore generated?
* Which customers have placed multiple orders?
* Which books are ordered most frequently?
* How much stock remains after fulfilling orders?

---

## 🗂️ Project Structure

```text
SQL_PROJECT/
│
├── Books.csv
├── Customers.csv
├── Orders.csv
└── SQL – Project Queries.sql
```

### Files

| File                        | Description                                                     |
| --------------------------- | --------------------------------------------------------------- |
| `Books.csv`                 | Book information and inventory data                             |
| `Customers.csv`             | Customer details                                                |
| `Orders.csv`                | Customer order information                                      |
| `SQL – Project Queries.sql` | Database creation, table creation, data import, and SQL queries |

---

## 🛠️ Technologies Used

* **PostgreSQL**
* **SQL**
* CSV datasets

---

## 🗃️ Database Schema

The project contains three main tables.

### 📖 Books

| Column           | Data Type     | Description         |
| ---------------- | ------------- | ------------------- |
| `Book_ID`        | SERIAL / INT  | Primary key         |
| `Title`          | VARCHAR(100)  | Book title          |
| `Author`         | VARCHAR(100)  | Book author         |
| `Genre`          | VARCHAR(50)   | Book genre          |
| `Published_Year` | INT           | Year of publication |
| `Price`          | NUMERIC(10,2) | Book price          |
| `Stock`          | INT           | Available stock     |

### 👥 Customers

| Column        | Data Type    | Description      |
| ------------- | ------------ | ---------------- |
| `Customer_ID` | SERIAL / INT | Primary key      |
| `Name`        | VARCHAR(100) | Customer name    |
| `Email`       | VARCHAR(100) | Customer email   |
| `Phone`       | VARCHAR(15)  | Customer phone   |
| `City`        | VARCHAR(50)  | Customer city    |
| `Country`     | VARCHAR(150) | Customer country |

### 🛒 Orders

| Column         | Data Type     | Description                         |
| -------------- | ------------- | ----------------------------------- |
| `Order_ID`     | SERIAL / INT  | Primary key                         |
| `Customer_ID`  | INT           | Foreign key referencing `Customers` |
| `Book_ID`      | INT           | Foreign key referencing `Books`     |
| `Order_Date`   | DATE          | Date of order                       |
| `Quantity`     | INT           | Number of books ordered             |
| `Total_Amount` | NUMERIC(10,2) | Total order amount                  |

### 🔗 Relationships

```text
Customers
    │
    │ Customer_ID
    ▼
 Orders
    │
    │ Book_ID
    ▼
 Books
```

* `Orders.Customer_ID` → `Customers.Customer_ID`
* `Orders.Book_ID` → `Books.Book_ID`

---

## ⚙️ Setup & Installation

### 1. Install PostgreSQL

Install PostgreSQL and open **pgAdmin** or **psql**.

### 2. Create the Database

Run:

```sql
CREATE DATABASE OnlineBookstore;
```

Then connect to the database:

```sql
\c OnlineBookstore;
```

### 3. Create the Tables

The SQL script contains the required `CREATE TABLE` statements for:

* `Books`
* `Customers`
* `Orders`

### 4. Import the CSV Data

Update the CSV file paths in the SQL script according to your local system.

For example:

```sql
COPY Books(Book_ID, Title, Author, Genre, Published_Year, Price, Stock)
FROM 'path/to/Books.csv'
CSV HEADER;
```

Similarly, import:

```text
Customers.csv
Orders.csv
```

> **Note:** The original SQL file contains a local Windows file path. You will need to change it before running the project on another computer.

---

# 📊 SQL Queries Covered

## 🟢 Basic SQL Queries

The project includes queries for:

1. Retrieve all books from the **Fiction** genre.
2. Find books published after **1950**.
3. List customers from **Canada**.
4. Retrieve orders placed during **November 2023**.
5. Calculate the total stock of books.
6. Find the most expensive book.
7. Find orders where the quantity is greater than 1.
8. Retrieve orders with a total amount greater than `$20`.
9. List all unique book genres.
10. Find the book with the lowest stock.
11. Calculate the total revenue generated from all orders.

### Example

```sql
SELECT *
FROM Books
WHERE Genre = 'Fiction';
```

---

# 🔥 Advanced SQL Queries

The project also contains more advanced analytical queries.

### 1. Total Books Sold by Genre

Uses:

* `JOIN`
* `SUM()`
* `GROUP BY`

```sql
SELECT b.Genre,
       SUM(o.Quantity) AS Total_Books_Sold
FROM Orders o
JOIN Books b
ON o.Book_ID = b.Book_ID
GROUP BY b.Genre;
```

### 2. Average Price of Fantasy Books

Uses:

* `AVG()`
* `WHERE`

```sql
SELECT AVG(Price) AS Average_Price
FROM Books
WHERE Genre = 'Fantasy';
```

### 3. Customers with at Least 2 Orders

Uses:

* `JOIN`
* `COUNT()`
* `GROUP BY`
* `HAVING`

```sql
SELECT o.Customer_ID,
       c.Name,
       COUNT(o.Order_ID) AS Order_Count
FROM Orders o
JOIN Customers c
ON o.Customer_ID = c.Customer_ID
GROUP BY o.Customer_ID, c.Name
HAVING COUNT(o.Order_ID) >= 2;
```

### 4. Most Frequently Ordered Book

Uses:

* `JOIN`
* `COUNT()`
* `GROUP BY`
* `ORDER BY`
* `LIMIT`

```sql
SELECT o.Book_ID,
       b.Title,
       COUNT(o.Order_ID) AS Order_Count
FROM Orders o
JOIN Books b
ON o.Book_ID = b.Book_ID
GROUP BY o.Book_ID, b.Title
ORDER BY Order_Count DESC
LIMIT 1;
```

### 5. Top 3 Most Expensive Fantasy Books

```sql
SELECT *
FROM Books
WHERE Genre = 'Fantasy'
ORDER BY Price DESC
LIMIT 3;
```

### 6. Total Books Sold by Author

```sql
SELECT b.Author,
       SUM(o.Quantity) AS Total_Books_Sold
FROM Orders o
JOIN Books b
ON o.Book_ID = b.Book_ID
GROUP BY b.Author;
```

### 7. Cities of Customers Spending More Than $30

```sql
SELECT DISTINCT c.City,
       o.Total_Amount
FROM Orders o
JOIN Customers c
ON o.Customer_ID = c.Customer_ID
WHERE o.Total_Amount > 30;
```

### 8. Customer Who Spent the Most

```sql
SELECT c.Customer_ID,
       c.Name,
       SUM(o.Total_Amount) AS Total_Spent
FROM Orders o
JOIN Customers c
ON o.Customer_ID = c.Customer_ID
GROUP BY c.Customer_ID, c.Name
ORDER BY Total_Spent DESC
LIMIT 1;
```

### 9. Remaining Stock After Orders

Uses:

* `LEFT JOIN`
* `SUM()`
* `COALESCE()`
* `GROUP BY`

```sql
SELECT b.Book_ID,
       b.Title,
       b.Stock,
       COALESCE(SUM(o.Quantity), 0) AS Order_Quantity,
       b.Stock - COALESCE(SUM(o.Quantity), 0) AS Remaining_Quantity
FROM Books b
LEFT JOIN Orders o
ON b.Book_ID = o.Book_ID
GROUP BY b.Book_ID
ORDER BY b.Book_ID;
```

---

## 🧠 SQL Concepts Practiced

This project covers several important SQL concepts:

* `CREATE DATABASE`
* `CREATE TABLE`
* Primary Keys
* Foreign Keys
* `COPY`
* `SELECT`
* `WHERE`
* `BETWEEN`
* `DISTINCT`
* `ORDER BY`
* `LIMIT`
* Aggregate Functions

  * `SUM()`
  * `AVG()`
  * `COUNT()`
* `GROUP BY`
* `HAVING`
* `INNER JOIN`
* `LEFT JOIN`
* `COALESCE()`
* Filtering and sorting
* Basic data analysis

---

## 🎯 Learning Objectives

The main goal of this project is to develop practical SQL skills by working with a relational database and answering business-oriented questions.

After completing this project, you should have a better understanding of:

* How relational databases are structured.
* How tables are connected using primary and foreign keys.
* How to retrieve and filter data.
* How to perform calculations using aggregate functions.
* How to combine information from multiple tables using joins.
* How to group and analyze data.
* How SQL can be used for real-world data analysis.

---

## 📌 Future Improvements

Some possible improvements for this project include:

* Add more complex subqueries.
* Add CTEs (`WITH` clauses).
* Add window functions such as `ROW_NUMBER()` and `DENSE_RANK()`.
* Create monthly and yearly revenue reports.
* Analyze best-selling genres and authors.
* Identify repeat customers.
* Add inventory alerts for low-stock books.
* Create SQL views for frequently used reports.
* Build a dashboard using Power BI or Tableau.

---

## 👨‍💻 Author

**ARI**

This project was created as a practical SQL learning and portfolio project focused on PostgreSQL and relational data analysis.

---

## ⭐ If You Found This Project Useful

If this project helped you learn SQL, consider giving the repository a ⭐ on GitHub!
