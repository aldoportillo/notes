# Relational Databases

A relational databases enforces a tabular like structure. Storing things in tables.

Columns are attributes. Rows are columns. The data is very structured due to the strict schemas.

Most relational databases support SQL, so Relational Databases and SQL databases are used synonymously. We can also perform powerful queries on our data!

In theory you can write a script to do these queries but they are expensive.

## ACID Transactions

- A - Atomicity
  - If a transaction consists of multiple sub operations, these sub operations count as a unit. If one fails, they both fail. If they both succeed, the unit is processed.
  - Think of a Zelle transfer.
- C - Consistency
  - Any transaction will abide by all rules in the DB and any future transaction will consider any past transaction to avoid stale data.
- I - Isolation
  - Multiple transactions can occur at the same time but they would be executed as if they were in a queue.
- D - Durability
  - When you make a transaction on the DB, the transaction is permanent.

## Database Index

Create auxillary data structure that is optimized for searching for a specific attribute in your table.

Types of Databases Indexes:

- Bitmap Indexes
- Reverse Indexes
- Dense Indexes

Example:

1. Create a sorted database index for the attribute you want and a pointer referencing the table with the record
2. Perform an O(log(n)) operation as opposed to a O(N) for a million record table

```sql
-- In relationalDB.sql

CREATE TABLE payments (
    customer_name varchar(128) NOT NULL,
    process_date date NOT NULL,
    amount int NOT NULL,
)

CREATE TABLE balances (
    username varchar(128) NOT NULL,
    balance int NOT NULL,
)

CREATE TABLE large_table (
    random_int int NOT NULL,
)

INSERT INTO payments (customer_name, process_date, amount) VALUES ('Alice', '2020-01-01', 100);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Bob', '2020-01-02', 200);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Charlie', '2020-01-03', 300);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Dan', '2020-01-04', 400);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Eve', '2020-01-05', 500);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Frank', '2020-01-06', 600);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Grace', '2020-01-07', 700);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Heidi', '2020-01-08', 800);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Ivan', '2020-01-09', 900);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Judy', '2020-01-10', 1000);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Mallory', '2020-01-11', 1100);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Oscar', '2020-02-12', 1200);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Peggy', '2020-02-13', 1300);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Trent', '2020-02-14', 1400);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Walter', '2020-02-15', 1500);
INSERT INTO payments (customer_name, process_date, amount) VALUES ('Mallory', '2020-02-16', 1600);

INSERT INTO balances (username, balance) VALUES ('Alice', 0);
INSERT INTO balances (username, balance) VALUES ('Bob', 400);

INSERT INTO large_table (random_int);
SELECT round(random() * 1000000) 
FROM generate_series(1, 10000000) s(i);


-- Examples to Run

/* Queries */

-- Sum number of payments for each user
SELECT customer_name, count(*) 
FROM payments
GROUP BY customer_name;
ORDER BY count DESC;

-- Sum the payment amount for each month

SELECT sum(amount), extract(year from process_date) as year, extract(month from process_date) as month
FROM payments
GROUP BY month, year
ORDER BY sum DESC;

-- Sum the payment amount for each month, for each user
SELECT customer_name, sum(amount), extract(year from process_date) as year, extract(month from process_date) as month
FROM payments
GROUP BY customer_name, month, year
ORDER BY customer_name DESC;

-- Find Largest Single User Payment for each month
SELECT max(amount), year, month
FROM (
    SELECT customer_name, sum(amount) as amount, extract(year from process_date) as year, extract(month from process_date) as month
    FROM payments
    GROUP BY customer_name, month, year
) AS monthly_sums
GROUP BY year, month;

/* Transactions */

-- Alice pays Bob 100
BEGIN TRANSACTION;
UPDATE balances SET balance = balance - 100 WHERE username = 'Alice';
UPDATE balances SET balance = balance + 100 WHERE username = 'Bob';
COMMIT;

-- Test Isolation by running these in parallel


/* Indexes */

-- Find 10 largest payments takes about 16 seconds
SELECT * FROM payments ORDER BY random_int DESC LIMIT 10;

-- Create index on ints table takes about 1 second
CREATE INDEX random_int_index ON large_table(random_int);
```

## Conclusion

A relational database's ACID like properties allow us to have **strong consistency** which keeps our data fresh. This is unlike **eventual consistency** in which reads might return a view of a stale system but guarantees that the state of the database will eventually reflect writes within a given time period.