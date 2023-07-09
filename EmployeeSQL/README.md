# SQL_Challenge

Data modeling, data engineering, and data analysis

1. Inspected the CSV files and did not find any duplicates
	• Departments.csv
	• Dept_emp.csv
	• Dept_manager.csv
	• Employees.csv
	• Salaries.csv
    Titles.csv

2. Created entity Relationship Diagram of the tables
https://app.quickdatabasediagrams.com/#/

Employees
-
emp_no INT PK
emp_title VARCHAR(20) FK >- Titles.title_ID
birth_date DATE
first_name VARCHAR(255)
last_name VARCHAR(255)
sex VARCHAR(10)
hire_date DATE      

Dept_Emp
-
emp_no INT PK FK >- Employees.emp_no
dept_no VARCHAR(10) FK >- Departments.dept_no

Dept_Manager
-
dept_no VARCHAR(10) FK >- Departments.dept_no
emp_no INT PK FK >- Employees.emp_no

Departments
-
dept_no PK
dept_name VARCHAR(20)
Titles
-
title_ID PK
title VARCHAR(20)

Salaries
-
emp_no INT FK >- Employees.emp_no
salaries INT

3. Created table schema for 6 csv files
CREATE TABLE employees (
	emp_no INT NOT NULL,
	emp_title VARCHAR(10) NOT NULL,
	birth_date DATE NOT NULL,
	first_name VARCHAR(255) NOT NULL,
	last_name VARCHAR(255)NOT NULL,
	sex VARCHAR(10) NOT NULL,
	hire_date DATE NOT NULL,
	PRIMARY KEY (emp_no)	
);

--View Table
SELECT * from employees;

CREATE TABLE departments (
	dept_no VARCHAR(10) NOT NULL,
	dept_name VARCHAR(20) NOT NULL,
	PRIMARY KEY (dept_no)
);

--View Table
SELECT * from departments;

CREATE TABLE dept_manager (
	dept_no VARCHAR(10) NOT NULL,
	emp_no INT NOT NULL,
	PRIMARY KEY (emp_no),
	FOREIGN KEY (dept_no) REFERENCES departments (dept_no)
	);

--View Table
SELECT * FROM dept_manager;

CREATE TABLE dept_emp (
	emp_no INT NOT NULL,
	dept_no VARCHAR(10) NOT NULL,
	PRIMARY KEY (emp_no, dept_no),
	FOREIGN KEY (dept_no) REFERENCES departments (dept_no)
);

--View Table
SELECT * FROM dept_emp;

CREATE TABLE titles (
	title_id VARCHAR(10) NOT NULL,
	title VARCHAR(20) NOT NULL,
	PRIMARY KEY (title_id)
	);
	
--View Table
SELECT * FROM titles;

CREATE TABLE salaries (
	emp_no INT NOT NULL,
	salary INT NOT NULL,
	PRIMARY KEY (emp_no)
	);

--View Table
SELECT * FROM salaries;

Queries
1. List the employee number, last name, first name, sex, and salary of each employee
--Use Employees and Salaries Tables

SELECT employees.emp_no, employees.last_name, employees.first_name, employees.sex,salaries.salary
FROM employees
INNER JOIN salaries ON
employees.emp_no = salaries.emp_no


--2. List the first name, last name, and hire date for the employees who were hired in 1986.
--Use Employees table
--RESOURCE: https://stackoverflow.com/questions/14208958/select-data-from-date-range-between-two-dates

--SELECT first_name, last_name, hire_date
SELECT first_name, last_name, hire_date
FROM employees WHERE
(hire_date BETWEEN '1986-01-01' and'1986-12-31');


--3. List the manager of each department along with their department number, 
     --department name, employee number, last name, and first name.
	--USE 3 TABLES: dept_manager table, departments table, employees table

--SELECT owners.owner_name,pet_names_new.pet_name, pet_names_new.type, service.service_type
--FROM owners
--INNER JOIN pet_names_new ON owners.ID = pet_names_new.owner_id
--INNER JOIN service ON service.id = pet_names_new.service_id;

--SELECT dept_manager.emp_no, dept_manager.dept_no, departments.dept_name
--FROM dept_manager
--INNER JOIN departments ON dept_manager.dept_no = departments.dept_no
--INNER JOIN employees on dept_manager.emp_no = employees.emp_no;

SELECT employees.emp_no, employees.last_name, employees.first_name
FROM employees
INNER JOIN dept_manager ON dept_manager.emp_no = employees.emp_no;

--4. List the department number for each employee along with that employee’s employee number, 
--last name, first name, and department name.
--dept_emp table, employee table, departments table

SELECT dept.dept_no, employees.last_name, employee.first_name. departments.dept_name



--5. List first name, last name, and sex of each employee whose first name is Hercules and 
--whose last name begins with the letter B.
SELECT first_name, last_name, sex
FROM employees
WHERE first_name = 'Hercules'
AND last_name LIKE 'B%';


--6.List each employee in the Sales department, including their employee number, last name, and first name.
--employee table, dept_emp, departments
SELECT emp_no, last_name, first_name
FROM employees
WHERE 


--7. List each employee in the Sales and Development departments, including their employee number, last name, first name, and department name.
-- use above, pulling in Development department


--8. List the frequency counts, in descending order, of all the employee last names (that is, how many employees share each last name).
SELECT COUNT(last_name) FROM employees
GROUP BY last_name
ORDER BY count DESC;