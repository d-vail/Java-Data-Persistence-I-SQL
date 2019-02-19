# Java Data Persistence I - RDBMS and SQL Basics

Goals of this module are to:
- Explain what SQL is and its advantages
- Explain what a Relational Database is and its core components
- Use SQLite Studio to create a SQLite database
- Use SQLite Studio to open an existing SQLite database
- Use SQLite Studio to add tables to a SQLite database
- Query data from a single table
- Query data from multiple tables

## Instructions

### 1. Query Practice

Surf to [SQL Try Editor at W3Schools.com](https://www.w3schools.com/Sql/tryit.asp?filename=trysql_select_top). Answer the following data queries. Keep track of the SQL you write by pasting it into this document under its appropriate header below.

#### Find all customers that live in London. Returns 6 records.

```` sql
SELECT *
FROM customers
WHERE city = "London"
````

#### Find all customers with postal code 1010. Returns 3 customers.

```` sql
SELECT *
FROM customers
WHERE postalcode = "1010"
````

#### Find the phone number for the supplier with the id 11. Should be (010) 9984510.

```` sql
SELECT phone
FROM suppliers
WHERE supplierid = 11
````

#### List orders descending by the order date. The order with date 1997-02-12 should be at the top.

```` sql
SELECT *
FROM orders
ORDER BY orderdate DESC
````

#### Find all suppliers who have names longer than 20 characters. You can use `length(SupplierName)` to get the length of the name. Returns 11 records.

```` sql
SELECT *
FROM suppliers
WHERE length(suppliername) > 20
````

#### Find all customers that include the word "market" in the name. Should return 4 records.

```` sql
SELECT *
FROM customers
WHERE customername LIKE "%market%"
````

#### Add a customer record for _"The Shire"_, the contact name is _"Bilbo Baggins"_ the address is _"1 Hobbit-Hole"_ in _"Bag End"_, postal code _"111"_ and the country is _"Middle Earth"_.

```` sql
INSERT INTO customers (customername, contactname, address, city, postalcode, country)
VALUES ("The Shire", "Bilbo Baggins", "1 Hobbit-Hole", "Bag End", "111", "Middle Earth")
````

#### Update _Bilbo Baggins_ record so that the postal code changes to _"11122"_.

```` sql
UPDATE customers
SET postalcode = "11122"
WHERE customerid = 92
````

#### List orders grouped by customer showing the number of orders per customer. _Rattlesnake Canyon Grocery_ should have 7 orders.

```` sql
SELECT c.customername, COUNT(c.customername) AS NumberOfOrders
FROM orders AS o
JOIN customers AS c
WHERE o.customerid = c.customerid
GROUP BY c.customername
````

#### List customers names and the number of orders per customer. Sort the list by number of orders in descending order. _Ernst Handel_ should be at the top with 10 orders followed by _QUICK-Stop_, _Rattlesnake Canyon Grocery_ and _Wartian Herkku_ with 7 orders each.

```` sql
SELECT c.customername, COUNT(c.customername) AS NumberOfOrders
FROM orders AS o
JOIN customers AS c
WHERE o.customerid = c.customerid
GROUP BY c.customername
ORDER BY NumberOfOrders DESC
````

#### List orders grouped by customer's city showing number of orders per city. Returns 58 Records with _Aachen_ showing 2 orders and _Albuquerque_ showing 7 orders.

```` sql
SELECT c.city, COUNT(c.city) AS NumberOfOrders
FROM orders AS o
JOIN customers AS c
WHERE o.customerid = c.customerid
GROUP BY city
````

#### Delete all customers that have no orders. Should delete 17 (or 18 if you haven't deleted the record added) records.

```` sql
DELETE FROM customers
WHERE customerid IN 
(
  SELECT c.customerid
  FROM customers AS c
  LEFT JOIN orders AS o
  ON c.customerid = o.customerid
  WHERE o.orderid IS NULL
)
````

### 2. Creating a Database in SQLite

Keep track of the code you write and paste at the end of this document

- Use [`SQLite Studio`](https://sqlitestudio.pl/index.rvt) to create a database, name it `budget.sqlite3`.
- Add an `accounts` table with the following _schema_:

  - `id`, numeric value with no decimal places that should autoincrement.
  - `name`, string, add whatever is necessary to make searching by name faster.
  - `budget` numeric value.

- Constraints
  - the `id` should be the primary key for the table.
  - account `name` should be unique.
  - account `budget` is required.
> This can be done with the CREATE TABLE clause