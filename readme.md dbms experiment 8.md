# EXPERIMENT – 8

## AIM:

To implement banking-style transactions using BEGIN, COMMIT, ROLLBACK, and SAVEPOINT, simulate partial failures, and demonstrate isolation levels and concurrency anomalies using two sessions.

## SOURCE CODE:

### 1. TABLE CREATING AND INSERTING RECORDS

```sql
CREATE TABLE Bank_Account (
    Account_ID INT PRIMARY KEY,
    Account_Holder VARCHAR(50),
    Balance DECIMAL(10,2)
);

INSERT INTO Bank_Account
VALUES
(101, 'Rahul Sharma', 50000.00),
(102, 'Priya Singh', 30000.00),
(103, 'Amit Kumar', 40000.00);

SELECT * FROM Bank_Account;
```

### 2. COMMIT TRANSACTION

```sql
START TRANSACTION;

UPDATE Bank_Account
SET Balance = Balance - 5000
WHERE Account_ID = 101;

UPDATE Bank_Account
SET Balance = Balance + 5000
WHERE Account_ID = 102;

COMMIT;

SELECT * FROM Bank_Account;
```

### 3. ROLLBACK TRANSACTION

```sql
START TRANSACTION;

UPDATE Bank_Account
SET Balance = Balance - 10000
WHERE Account_ID = 101;

UPDATE Bank_Account
SET Balance = Balance + 10000
WHERE Account_ID = 103;

ROLLBACK;

SELECT * FROM Bank_Account;
```

### 4. SAVEPOINT AND ROLLBACK TO SAVEPOINT

```sql
START TRANSACTION;

UPDATE Bank_Account
SET Balance = Balance - 3000
WHERE Account_ID = 101;

SAVEPOINT transfer_point;

UPDATE Bank_Account
SET Balance = Balance + 3000
WHERE Account_ID = 102;

ROLLBACK TO transfer_point;

COMMIT;

SELECT * FROM Bank_Account;
```

### 5. PARTIAL ROLLBACK USING SAVEPOINT

```sql
START TRANSACTION;

UPDATE Bank_Account
SET Balance = Balance - 2000
WHERE Account_ID = 101;

SAVEPOINT before_second_transfer;

UPDATE Bank_Account
SET Balance = Balance + 2000
WHERE Account_ID = 102;

ROLLBACK TO before_second_transfer;

UPDATE Bank_Account
SET Balance = Balance + 2000
WHERE Account_ID = 103;

COMMIT;

SELECT * FROM Bank_Account;
```

### 6. READ COMMITTED ISOLATION LEVEL

```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;

START TRANSACTION;

SELECT *
FROM Bank_Account
WHERE Account_ID = 101;

COMMIT;
```

### 7. REPEATABLE READ ISOLATION LEVEL

```sql
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;

START TRANSACTION;

SELECT *
FROM Bank_Account
WHERE Account_ID = 101;

COMMIT;
```

### 8. SERIALIZABLE ISOLATION LEVEL

```sql
SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;

START TRANSACTION;

SELECT *
FROM Bank_Account
WHERE Account_ID = 101;

COMMIT;
```

### 9. TRANSACTION IN SESSION 1

```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;

START TRANSACTION;

SELECT Balance
FROM Bank_Account
WHERE Account_ID = 101;

UPDATE Bank_Account
SET Balance = Balance - 5000
WHERE Account_ID = 101;

COMMIT;
```

### 10. TRANSACTION IN SESSION 2

```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;

START TRANSACTION;

SELECT Balance
FROM Bank_Account
WHERE Account_ID = 101;

UPDATE Bank_Account
SET Balance = Balance - 3000
WHERE Account_ID = 101;

COMMIT;

SELECT *
FROM Bank_Account
WHERE Account_ID = 101;
```

## OUTPUT:

The output displays the bank account records after executing COMMIT, ROLLBACK, SAVEPOINT, and different transaction isolation levels.

## RESULT:

Thus, banking-style transactions were successfully implemented using BEGIN, COMMIT, ROLLBACK, and SAVEPOINT. Different transaction isolation levels were demonstrated using SQL queries.
