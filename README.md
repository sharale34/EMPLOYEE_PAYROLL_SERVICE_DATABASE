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
