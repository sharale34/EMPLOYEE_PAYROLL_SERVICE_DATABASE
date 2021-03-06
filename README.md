# Welcome to the Employee Payroll Service Database

## UC1 - Create database for payroll_service
```create database payroll_service;```

### See the DB created using show database query
```show databases;```

### Go to the database created
```use payroll_service;```

## UC2 - Create a employee payroll table in the payroll service
```CREATE TABLE employee_payroll
    -> (
    ->   id                  INT unsigned NOT NULL AUTO_INCREMENT,
    ->   name                VARCHAR(150) NOT NULL,
    ->   salary              Double NOT NULL,
    ->   start               DATE NOT NULL,
    ->   PRIMARY KEY         (id)
    -> );
```
### See the table by using database query
```DESCRIBE employee_payroll;```

## UC3 - Ability to create employee payroll data in the payroll service
```INSERT INTO employee_payroll(name , salary , start) VALUES
    -> ('Bill',1000000.00,'2018-01-03'),
    -> ('Terisa',2000000.00,'2019-11-13'),
    -> ('Charlie',3000000.00,'2020-05-21');
```
## UC4 - Ability to retrieve all the employee payroll data
```SELECT * FROM employee_payroll;```

## UC5 - Ability to retrieve salary of particular person and employees who have joined in a particular data range
### Viewing salary of particular person
```SELECT salary FROM employee_payroll WHERE name='Charlie';```

### Checking employees who joined at particular date range
```SELECT * FROM employee_payroll WHERE start BETWEEN CAST('2019-01-01' AS DATE) AND DATE(NOW());```

## UC6- Ability to add gender to employee payroll table
### Adding gender
```ALTER TABLE employee_payroll ADD gender CHAR(1) AFTER name;```

### Setting F gender for female employees
```UPDATE employee_payroll SET gender = 'F' WHERE name='Terisa' or name='Deeksha';```

### Setting M gender for male employees
```UPDATE employee_payroll SET gender='M' WHERE name='Bill' or name='Charlie';```

### Viewing gender
```SELECT * FROM employee_payroll;```

## UC7 - Ability to Find Sum, Average, Min, Max, Count
### Total salary according to gender
```SELECT gender, SUM(salary) FROM employee_payroll GROUP BY gender;```

### Average salary according to gender
```SELECT gender, AVG(salary) FROM employee_payroll GROUP BY gender;```

### Minimum salary according to gender
```SELECT gender, MIN(salary) FROM employee_payroll GROUP BY gender;```

### Maximum salary according to gender
```SELECT gender, MAX(salary) FROM employee_payroll GROUP BY gender;```

### Count of employees according to gender
```SELECT gender, COUNT(salary) FROM employee_payroll GROUP BY gender;```

## UC8 - Ability to extend employee_payroll to add employee phone, address and department
```
ALTER TABLE employee_payroll ADD phone BIGINT AFTER name;
ALTER TABLE employee_payroll ADD address VARCHAR(250) AFTER phone;
ALTER TABLE employee_payroll ADD department VARCHAR(150) NOT NULL AFTER address;
UPDATE employee_payroll SET address='hyderabad';
```
## UC9 - Ability to extend employee_payroll to have Basic Pay, Deductions, Taxable Pay, Income Tax and Net Pay
```
ALTER TABLE employee_payroll RENAME COLUMN salary TO basic_pay;
ALTER TABLE employee_payroll ADD deductions DOUBLE NOT NULL AFTER basic_pay;
ALTER TABLE employee_payroll ADD taxable_pay DOUBLE NOT NULL AFTER deductions;
ALTER TABLE employee_payroll ADD income_tax DOUBLE NOT NULL AFTER taxable_pay;
ALTER TABLE employee_payroll ADD net_pay DOUBLE NOT NULL AFTER taxable_pay;
DESCRIBE employee_payroll;
```
## UC10 - Ability to make Terisa as part of Sales and Marketing department
```
UPDATE employee_payroll SET department='Sales' WHERE name='Terisa';
INSERT INTO employee_payroll(name,phone_number,address,department,gender,basic_pay,deductions,taxable_pay,tax,net_pay,start) VALUES
    -> ('Terisa','9494118273','hyderabad','Marketing','F','2000000','1000000','2000000','500000','1500000','2018-03-08');
SELECT * FROM employee_payroll WHERE name='Terisa';
```
## UC11 - Implement the ER Diagram into Payroll Service DataBase
```
CREATE TABLE company
    -> (
    -> company_id      INT NOT NULL PRIMARY KEY,
    -> company_name    VARCHAR(150) NOT NULL
    -> );
CREATE TABLE employee_details
    -> (
    -> employee_id     INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    -> company_id      INT NOT NULL,
    -> FOREIGN KEY     (company_id) REFERENCES company (company_id),
    -> name            VARCHAR(150) NOT NULL,
    -> phone_number    BIGINT(15) NOT NULL,
    -> address         VARCHAR(250) NOT NULL,
    -> gender          CHAR(1) NOT NULL,
    -> start_date      DATE NOT NULL
    -> );
CREATE TABLE payroll
    -> (
    -> employee_id     INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    -> FOREIGN KEY     (employee_id) REFERENCES employee (employee_id),
    -> basic_pay       DOUBLE NOT NULL,
    -> deductions      DOUBLE NOT NULL,
    -> taxable_pay     DOUBLE NOT NULL,
    -> tax             DOUBLE NOT NULL,
    -> net_pay         DOUBLE NOT NULL
    -> );
CREATE TABLE department
    -> (
    -> department_id   INT NOT NULL PRIMARY KEY,
    -> department_name VARCHAR(150) NOT NULL
    -> );
CREATE TABLE employee_department
    -> (
    -> employee_id     INT UNSIGNED NOT NULL AUTO_INCREMENT,
    -> FOREIGN KEY     (employee_id) REFERENCES employee (employee_id),
    -> department_id   INT NOT NULL,
    -> FOREIGN KEY     (department_id) REFERENCES department (department_id)
    -> );

### Inserting values into the tables created

INSERT INTO company VALUES
    -> (1,'Google'),
    -> (2,'Microsoft'),
    -> (3,'Capgemini');

INSERT INTO employee VALUES
    -> (101, 1, 'Bill', '9494118273', 'Hyderabad', 'M', '2018-01-01'),
    -> (102, 2, 'Terisa', '7207020464', 'Nizamabad', 'F', '2019-11-11'),
    -> (103, 3, 'Charlie', '9440293758', 'Chennai', 'M', '2020-05-21');
INSERT INTO payroll VALUES
    -> ('101', 4000000.00, 1000000.00, 3000000.00, 500000.00, 2500000.00),
    -> ('102', 5000000.00, 1500000.00, 3500000.00, 1000000.00, 2500000.00),
    -> ('103', 6000000.00, 2000000.00, 4000000.00, 1500000.00, 2500000.00);
INSERT INTO department VALUES
    -> (110,'Sales'),
    -> (111,'Marketing'),
    -> (112,'Finance');
INSERT INTO employee_department VALUES
    -> (101,110),
    -> (102,111),
    -> (103,112),
    -> (102,110);
```
## UC12 - Ability to ensure all retrieve queries done are working with new table structure
```
SELECT * FROM company;
SELECT * FROM employee_details;
SELECT * FROM payroll;
SELECT * FROM department;
SELECT * FROM employee_department;
SELECT salary FROM payroll WHERE name = 'Bill';
SELECT * FROM payroll WHERE start BETWEEN CAST('2018-01-01' AS DATE) and DATE(NOW());
SELECT SUM(p.net_pay), e.gender FROM employee e LEFT JOIN payroll p ON p.employee_id = e.employee_id GROUP BY e.gender;
SELECT AVG(p.net_pay), e.gender FROM employee e LEFT JOIN payroll p ON p.employee_id = e.employee_id GROUP BY e.gender;
SELECT MIN(p.net_pay), e.gender FROM employee e LEFT JOIN payroll p ON p.employee_id = e.employee_id GROUP BY e.gender;
SELECT MAX(p.net_pay), e.gender FROM employee e LEFT JOIN payroll p ON p.employee_id = e.employee_id GROUP BY e.gender;
SELECT COUNT(p.net_pay), e.gender FROM employee e LEFT JOIN payroll p ON p.employee_id = e.employee_id GROUP BY e.gender;
```
