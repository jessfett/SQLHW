# SQL Homework 9 - Employee Database: A Mystery in Two Parts


## Process
The assignment consisted of analyzing the remaining employee database for Pewlett Hackard for 1980s and 1990s, which includes only 6 CSV files. 


1. Data Modeling/Data Engineering
2. Data Analysis


I analyzed the remaining CSV files and created a relationship database, which can be found in the [ERD Folder](https://github.com/jessfett/HW9/tree/master/ERD). After that, the tables were created in an SQL file using PGAdmin4 and analyzed for the required questions. 

## Data Modeling/Data Engineering

First task was to create an ERD in QuickDBD. 

![ERD - Employees](https://github.com/jessfett/HW9/blob/master/ERD/ERD%20-%20Employees.png)

The tables were created in PGAdmin4. After inspecting the CSV files and creating an ERD, the proper Primary Keys and Foreign Keys were utilized to create tables that were able to be analyzed. 

```
--Drop if existing
DROP TABLE IF EXISTS departments CASCADE;
DROP TABLE IF EXISTS dept_emp CASCADE;
DROP TABLE IF EXISTS dept_manager CASCADE;
DROP TABLE IF EXISTS employees CASCADE;
DROP TABLE IF EXISTS salaries CASCADE;
DROP TABLE IF EXISTS titles CASCADE;

--Create neccessary tables
CREATE TABLE "departments" (
	"dept_no" VARCHAR NOT NULL,
	"dept_name" VARCHAR NOT NULL,
	CONSTRAINT "pk_departments" PRIMARY KEY (
	"dept_no"));
	
SELECT * FROM departments;

CREATE TABLE "dept_emp" (
	"emp_no" INT NOT NULL,
	"dept_no" VARCHAR NOT NULL);
	
SELECT * FROM dept_emp;	
	
CREATE TABLE "dept_manager"(
	"dept_no" VARCHAR NOT NULL, 
	"emp_no" INT NOT NULL);

SELECT * FROM dept_manager;
	
CREATE TABLE "employees" (
	"emp_no" INT NOT NULL,
	emp_title_id VARCHAR NOT NULL,
	"birth_date" DATE NOT NULL,
	"first_name" VARCHAR NOT NULL,
	"last_name" VARCHAR NOT NULL,
	"sex" VARCHAR NOT NULL,
	"hire_date" DATE NOT NULL,
	CONSTRAINT "pk_employees" PRIMARY KEY (
		"emp_no"));
		
SELECT * FROM employees;		
		
CREATE TABLE "salaries" (
	"emp_no" INT NOT NULL,
	"salary" INT NOT NULL);

SELECT * FROM salaries; 

CREATE TABLE "titles" (
	"title_id" VARCHAR NOT NULL,
	"title" VARCHAR NOT NULL);
	
SELECT * FROM titles;
```


## Data Analysis

After completing the database and relationships, I analyzed the information for the following:
1. List the following details of each employee: employee number, last name, first name, sex, and salary.

```
SELECT employees.emp_no,
  employees.last_name,
  employees.first_name,
  employees.sex,
  salaries.salary
FROM employees
INNER JOIN salaries ON
employees.emp_no = salaries.emp_no;
```

2. List first name, last name, and hire date for employees who were hired in 1986.

```
SELECT last_name, first_name, hire_date
FROM employees
WHERE hire_date BETWEEN '1986-01-01' AND '1986-12-31';
```

3. List the manager of each department with the following information: department number, department name, the manager's employee number, last name, first name.

```
SELECT departments.dept_no, departments.dept_name, dept_manager.emp_no, employees.last_name, employees.first_name
FROM departments
JOIN dept_manager
ON departments.dept_no = dept_manager.dept_no
JOIN employees
ON dept_manager.emp_no = employees.emp_no;
```

4. List the department of each employee with the following information: employee number, last name, first name, and department name.

```
SELECT employees.emp_no, employees.last_name, employees.first_name, departments.dept_name
FROM employees
JOIN dept_emp
ON dept_emp.emp_no = employees.emp_no
JOIN departments
ON departments.dept_no = dept_emp.dept_no;
```

5. List first name, last name, and sex for employees whose first name is "Hercules" and last names begin with "B."

```
SELECT first_name, last_name, sex
FROM employees
WHERE first_name = 'Hercules' AND last_name LIKE 'B%';
```


6. List all employees in the Sales department, including their employee number, last name, first name, and department name.

```
SELECT employees.emp_no, employees.last_name, employees.first_name, departments.dept_name
FROM employees
JOIN dept_emp
ON dept_emp.emp_no = employees.emp_no
JOIN departments
ON departments.dept_no = dept_emp.dept_no
WHERE departments.dept_name = 'Sales'
```

7. List all employees in the Sales and Development departments, including their employee number, last name, first name, and department name.

```
SELECT employees.emp_no, employees.last_name, employees.first_name, departments.dept_name
FROM employees
JOIN dept_emp
ON dept_emp.emp_no = employees.emp_no
JOIN departments
ON departments.dept_no = dept_emp.dept_no
WHERE departments.dept_name = 'Sales'
OR departments.dept_name = 'Development'
```


8. In descending order, list the frequency count of employee last names, i.e., how many employees share each last name.

```
SELECT last_name, COUNT(last_name) AS "name count"
FROM employees
GROUP BY last_name
ORDER BY "name count" DESC;
```

