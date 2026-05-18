# Practical 4: AWS RDS MySQL operations (DDL, DML, joins, filters, aggregation)

## Goal

Create an RDS MySQL instance and run database creation, inserts, imports, joins, filtering, and aggregations.

## Prerequisites

- AWS account
- MySQL client (MySQL Workbench or mysql CLI)
- Internet access

## Part 1: Create RDS MySQL instance (AWS Console)

1. Open AWS Console -> RDS -> Create database.
2. Engine: MySQL.
3. Template: Free tier.
4. DB instance identifier: mydb-instance.
5. Master username: admin.
6. Master password: choose a strong password and keep it safe.
7. Connectivity:
   - Public access: Yes (for lab access)
   - VPC security group: allow inbound 3306 from your IP.
8. Create database and wait for status Available.
9. Copy the DB endpoint.

## Part 2: Connect to RDS

### Option A: MySQL Workbench

- Create a new connection using:
  - Hostname: <endpoint>
  - Port: 3306
  - Username: admin
  - Password: your password

### Option B: mysql CLI (Windows or EC2)

```
mysql -h <endpoint> -u admin -p
```

## Part 3: Create database and tables

```
CREATE DATABASE company_db;
USE company_db;

CREATE TABLE employees (
  emp_id INT PRIMARY KEY,
  name VARCHAR(50),
  department VARCHAR(50),
  salary INT
);

CREATE TABLE departments (
  dept_id INT PRIMARY KEY,
  dept_name VARCHAR(50)
);
```

## Part 4: Insert data

```
INSERT INTO employees VALUES
  (1, 'Amit', 'IT', 50000),
  (2, 'Neha', 'HR', 40000),
  (3, 'Viraj', 'IT', 60000),
  (4, 'Pallavi', 'Finance', 45000);

INSERT INTO departments VALUES
  (1, 'IT'),
  (2, 'HR'),
  (3, 'Finance');
```

## Part 5: Import data (CSV)

Create employees.csv on your local machine:

```
emp_id,name,department,salary
5,Anil,IT,52000
6,Pooja,HR,43000
```

From mysql CLI:

```
mysql --local-infile=1 -h <endpoint> -u admin -p
```

Then in MySQL:

```
LOAD DATA LOCAL INFILE 'D:/Me/College/sweetu/CCL/4. RDS/employees.csv'
INTO TABLE employees
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

## Part 6: Select and filter

```
SELECT * FROM employees;
SELECT name, salary FROM employees;
SELECT * FROM employees WHERE department = 'IT';
SELECT * FROM employees WHERE salary > 45000;
```

## Part 7: Join

```
SELECT e.name, d.dept_name
FROM employees e
JOIN departments d
ON e.department = d.dept_name;
```

## Part 8: Aggregations

```
SELECT COUNT(*) FROM employees;
SELECT department, AVG(salary) FROM employees GROUP BY department;
SELECT MAX(salary) FROM employees;
SELECT MIN(salary) FROM employees;
```

## Cleanup

- Delete the RDS instance when done to avoid charges.
