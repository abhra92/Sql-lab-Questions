I'll create a well-structured markdown file based on the DBMS lab practical questions you've provided.

````markdown name=DBMS_Lab_Practical_Questions_BCAC401.md
# DBMS Lab Practical Questions (BCAC401)

This document contains five sets of SQL practice questions for the DBMS Lab course (BCAC401). Each set includes 10 questions with SQL solutions and categories.

## Table Structure

The questions are based on the following schema:


CREATE TABLE emp (
  empno     INT PRIMARY KEY,
  ename     VARCHAR(50),
  job       VARCHAR(30),
  mgr       INT,
  hiredate  DATE,
  sal       DECIMAL(10,2),
  comm      DECIMAL(10,2),
  deptno    INT
);



CREATE TABLE dept (
  deptno    INT PRIMARY KEY,
  dname     VARCHAR(50),
  loc       VARCHAR(50)
);

````

Sample data is included in both tables, covering various employees, managers, departments, and locations.

## Set 1 (Questions 1-10)

### 1. Retrieve all employees and their respective department names using a natural join

```sql
SELECT E.ENAME, D.DNAME
FROM emp E
JOIN dept D ON E.DEPTNO = D.DEPTNO;
```

**Category**: Join

### 2. Find the names of employees who are managers

```sql
SELECT DISTINCT E.ENAME
FROM EMP E
WHERE E.EMPNO IN (SELECT MGR FROM EMP WHERE MGR IS NOT NULL);
```

**Category**: Filtering

### 3. List all employees along with their department locations

```sql
SELECT e.ename, d.loc
FROM emp e
JOIN dept d ON e.deptno = d.deptno;
```

**Category**: Join

### 4. Retrieve the names of employees who earn a commission (comm) greater than their salary (sal)

```sql
SELECT e.ename
FROM emp e
WHERE e.comm > e.sal;
```

**Category**: Condition

### 5. Find the department name and location for each employee

```sql
SELECT e.ename, d.dname, d.loc
FROM emp e
JOIN dept d ON e.deptno = d.deptno;
```

**Category**: Join

### 6. List the employees who work in the "SALES" department

```sql
SELECT e.ename
FROM emp e
JOIN dept d ON e.deptno = d.deptno
WHERE d.dname = 'SALES';
```

**Category**: Condition

### 7. Retrieve all employees hired after January 1, 1985

```sql
SELECT e.ename
FROM emp e
WHERE e.hiredate > TO_DATE('01-JAN-1985', 'DD-MON-YYYY');
```

**Category**: Date Filter

### 8. Find the employees who work in a location containing the word 'DALLAS'

```sql
SELECT e.ename
FROM emp e
JOIN dept d ON e.deptno = d.deptno
WHERE d.loc LIKE '%DALLAS%';
```

**Category**: Pattern Matching

### 9. List the department names along with the total number of employees in each department

```sql
SELECT d.dname, COUNT(e.empno)
FROM emp e
JOIN dept d ON e.deptno = d.deptno
GROUP BY d.dname;
```

**Category**: Group By

### 10. Retrieve the names of employees who work under a specific manager

```sql
SELECT e.ename
FROM emp e
WHERE e.mgr = 7839;
```

**Category**: Self Join

## Set 2 (Questions 11-20)

### 11. Retrieve the total salary paid in each department, ordered by department number

```sql
11. SELECT d.deptno, d.dname, SUM(e.sal) AS total_salary
FROM emp e
JOIN dept d ON e.deptno = d.deptno
GROUP By d.deptno, d.dname
ORDER BY d.deptno;
```

**Category**: Aggregation

### 12. Find the department names where the average salary of employees exceeds 5000

```sql
SELECT d.dname, AVG(e.sal)
FROM emp e
JOIN dept d ON e.deptno = d.deptno
GROUP BY d.dname
HAVING AVG(e.sal) > 5000;
```

**Category**: Group By

### 13. List the names of employees whose job titles contain the substring "CLERK"

```sql
SELECT e.ename
FROM emp e
WHERE e.job LIKE '%CLERK%';
```

**Category**: Pattern Matching

### 14. Retrieve the department name and the maximum salary in each department where the maximum salary exceeds 8000

```sql
SELECT d.dname, MAX(e.sal) AS Max_Salary
FROM emp e
JOIN dept d ON e.deptno = d.deptno
GROUP BY d.dname
HAVING MAX(e.sal) > 8000;
```

**Category**: Aggregation with HAVING

### 15. List the employees grouped by department, showing the total salary for each group

```sql
SELECT d.dname, SUM(e.sal) AS Total_Salary
FROM emp e
JOIN dept d ON e.deptno = d.deptno
GROUP BY d.dname;
```

**Category**: Group By

### 16. Retrieve the department names and the count of employees, but only include departments with more than 3 employees

```sql
SELECT d.dname, COUNT(e.empno) AS Emp_Count
FROM emp e
JOIN dept d ON e.deptno = d.deptno
GROUP BY d.dname
HAVING COUNT(e.empno) > 3;
```

**Category**: Group By + HAVING

### 17. Find the names of employees and their department names for employees earning between 4000 and 6000, ordered by their salary

```sql
SELECT e.ename, d.dname, e.sal
FROM emp e
JOIN dept d ON e.deptno = d.deptno
WHERE e.sal BETWEEN 4000 AND 6000
ORDER BY e.sal;
```

**Category**: Condition + Sorting

### 18. Retrieve the department names and their total salary for departments with "A" in the department name

```sql
SELECT d.dname, SUM(e.sal) AS Total_Salary
FROM emp e
JOIN dept d ON e.deptno = d.deptno
WHERE d.dname LIKE '%A%'
GROUP BY d.dname;
```

**Category**: Pattern + Aggregation

### 19. List the employees whose names start with the letter "S" and display their department details

```sql
SELECT e.ename, d.dname, d.loc
FROM emp e
JOIN dept d ON e.deptno = d.deptno
WHERE e.ename LIKE 'S%';
```

**Category**: Pattern + Join

### 20. Retrieve the department name, job title, and average salary for each job category in a department

```sql
SELECT d.dname, e.job, AVG(e.sal) AS Avg_Salary
FROM emp e
JOIN dept d ON e.deptno = d.deptno
GROUP BY d.dname, e.job;
```

**Category**: Multi-level Grouping

## Set 3 (Questions 21-30)

### 21. Retrieve the names of employees who do not work under any manager

```sql
SELECT ename
FROM emp
WHERE mgr IS NULL;
```

**Category**: Null Check

### 22. Display department-wise highest and lowest salary with employee name

```sql
SELECT e.deptno, e.ename, e.sal
FROM emp e
JOIN (
  SELECT deptno, MAX(sal) AS max_sal, MIN(sal) AS min_sal
  FROM emp
  GROUP BY deptno
) s ON e.deptno = s.deptno AND (e.sal = s.max_sal OR e.sal = s.min_sal);
```

**Category**: Aggregation + Join

### 23. Count the number of managers in each department

```sql
SELECT deptno, COUNT(DISTINCT empno) AS No_of_Managers
FROM emp
WHERE empno IN (SELECT DISTINCT mgr FROM emp WHERE mgr IS NOT NULL)
GROUP BY deptno;
```

**Category**: Group By

### 24. Retrieve employee names and their joining year using date functions

```sql
SELECT ename, YEAR(hiredate) AS joining_year
FROM emp;
```

**Category**: Date Function

### 25. Find departments where no employee has a commission

```sql
SELECT d.dname
FROM dept d
WHERE d.deptno NOT IN (
  SELECT DISTINCT deptno FROM emp WHERE comm IS NOT NULL AND comm > 0
);
```

**Category**: Subquery

### 26. Show the second highest salary in each department

```sql
SELECT e1.deptno, MAX(e1.sal) AS second_highest_sal
FROM emp e1
WHERE e1.sal < (
  SELECT MAX(e2.sal) FROM emp e2 WHERE e2.deptno = e1.deptno
)
GROUP BY e1.deptno;
```

**Category**: Advanced Query

### 27. List employees whose salary is above the department average

```sql
SELECT e.ename, e.sal, e.deptno
FROM emp e
JOIN (
  SELECT deptno, AVG(sal) AS avg_sal
  FROM emp
  GROUP BY deptno
) d ON e.deptno = d.deptno
WHERE e.sal > d.avg_sal;
```

**Category**: Subquery

### 28. Retrieve the department(s) with the lowest total salary

```sql
SELECT deptno, SUM(sal) AS Total_Salary
FROM emp
GROUP BY deptno
HAVING SUM(sal) = (
  SELECT MIN(Salary_sum) FROM (
    SELECT SUM(sal) AS Salary_sum FROM emp GROUP BY deptno
  ) AS dept_sums
);
```

**Category**: Aggregation + Subquery

### 29. List employee names along with their length using string function

```sql
SELECT ename, LENGTH(ename) AS name_length
FROM emp;
```

**Category**: String Function

### 30. Display the employee names in reverse order and their ASCII values

```sql
SELECT ename,
       REVERSE(ename) AS reversed_name,
       ASCII(SUBSTR(ename, 1, 1)) AS ascii_first_letter
FROM emp;
```

**Category**: String Function

## Set 4 (Questions 31-40)

### 31. Find the names of employees whose salary is more than the average salary of all employees

```sql
SELECT ename, sal
FROM emp
WHERE sal > (SELECT AVG(sal) FROM emp);
```

**Category**: Subquery

### 32. Retrieve the list of departments where no employee is assigned

```sql
SELECT dname, deptno
FROM dept
WHERE deptno NOT IN (SELECT DISTINCT deptno FROM emp);
```

**Category**: Outer Join

### 33. Show all employees who were hired in the month of December

```sql
SELECT ename, hiredate
FROM emp
WHERE TO_CHAR(hiredate, 'MM') = '12';
```

**Category**: Date Filter

### 34. Display employee names along with their job title and length of service in years

```sql
SELECT ename, job, TIMESTAMPDIFF(YEAR, hiredate, CURDATE()) AS years_of_service
FROM emp;
```

**Category**: Date Calculation

### 35. Find the employees who have the same job title as their manager

```sql
SELECT e.ename AS employee_name, e.job AS job_title, m.ename AS manager_name
FROM emp e
JOIN emp m ON e.mgr = m.empno
WHERE e.job = m.job;
```

**Category**: Self Join

### 36. Retrieve department-wise average, minimum, and maximum salary

```sql
SELECT deptno,
       AVG(sal) AS avg_salary,
       MIN(sal) AS min_salary,
       MAX(sal) AS max_salary
FROM emp
GROUP BY deptno;
```

**Category**: Aggregation

### 37. Show the employees with the highest commission in each department

```sql
SELECT E1.*
FROM emp E1
JOIN (
  SELECT deptno, MAX(comm) AS max_comm
  FROM emp
  GROUP BY deptno
) E2 ON E1.deptno = E2.deptno AND E1.comm = E2.max_comm;
```

**Category**: Aggregation + Join

### 38. Find the list of managers who manage more than 3 employees

```sql
SELECT m.empno, m.ename, COUNT(e.empno) AS No_of_Employees
FROM emp e
JOIN emp m ON e.mgr = m.empno
GROUP BY m.empno, m.ename
HAVING COUNT(e.empno) > 3;
```

**Category**: Group By + Filter

### 39. Display employees whose name ends with the letter 'N'

```sql
SELECT *
FROM emp
WHERE ename LIKE '%N';
```

**Category**: Pattern Matching

### 40. List the department with the highest number of employees

```sql
SELECT deptno, COUNT(*) AS No_of_Employees
FROM emp
GROUP BY deptno
ORDER BY No_of_Employees DESC
LIMIT 1;
```

**Category**: Aggregation + Subquery

## Set 5 (Questions 41-50)

### 41. Retrieve the employees who do not receive any commission

```sql
SELECT *
FROM emp
WHERE comm IS NULL OR comm = 0;
```

**Category**: Null/Zero Check

### 42. Find employees who joined between two given dates

```sql
SELECT *
FROM emp
WHERE hiredate BETWEEN TO_DATE('01-JAN-1985','DD-MON-YYYY') AND TO_DATE('31-DEC-1987','DD-MON-YYYY');
```

**Category**: Date Filter

### 43. Retrieve employee details along with department and manager name using multiple joins

```sql
SELECT e.empno,
       e.ename AS employee_name,
       e.job AS job_title,
       d.dname AS department_name,
       d.loc AS location,
       m.ename AS manager_name
FROM emp e
LEFT JOIN dept d ON e.deptno = d.deptno
LEFT JOIN emp m ON e.mgr = m.empno;
```

**Category**: Multiple Joins

### 44. Display all employee names in lowercase and uppercase

```sql
SELECT ename,
       LOWER(ename) AS lowercase_name,
       UPPER(ename) AS uppercase_name
FROM emp;
```

**Category**: String Function

### 45. Show department name and total salary only for departments located in 'NEW YORK'

```sql
SELECT d.dname, SUM(e.sal) AS total_salary
FROM emp e
JOIN dept d ON e.deptno = d.deptno
WHERE d.loc = 'NEW YORK'
GROUP BY d.dname;
```

**Category**: Join + Filter

### 46. Find the total salary paid to employees whose names contain the letter 'A'

```sql
SELECT SUM(sal) AS total_salary
FROM emp
WHERE ename LIKE '%A%';
```

**Category**: Pattern + Aggregation

### 47. Retrieve names of employees who have exactly 5 characters in their names

```sql
SELECT *
FROM emp
WHERE LENGTH(ename) = 5;
```

**Category**: String Function

### 48. Find the youngest employee based on hire date

```sql
SELECT *
FROM emp
ORDER BY hiredate DESC
LIMIT 1;
```

**Category**: Aggregation

### 49. Display department-wise job title counts

```sql
SELECT deptno, job, COUNT(*) AS job_count
FROM emp
GROUP BY deptno, job;
```

**Category**: Group By

### 50. List employees along with the number of characters in their job title

```sql
SELECT ename, job, LENGTH(job) AS job_length
FROM emp;
```

**Category**: String Function

## SQL Concepts Covered

This question set covers the following SQL concepts:

1. **Basic Querying**

   - SELECT, FROM, WHERE clauses
   - Joins (INNER JOIN, LEFT JOIN)
   - Table aliases

2. **Filtering & Sorting**

   - WHERE conditions (comparison operators, BETWEEN, LIKE)
   - Pattern matching with LIKE and wildcards
   - ORDER BY for sorting results
   - LIMIT clause

3. **Aggregation**

   - Aggregate functions (COUNT, SUM, AVG, MIN, MAX)
   - GROUP BY for data grouping
   - HAVING for filtering grouped data

4. **Joins & Relationships**

   - Inner joins
   - Left joins
   - Self-joins
   - Multiple table joins

5. **Subqueries**

   - Subqueries in SELECT
   - Subqueries in WHERE
   - Nested subqueries
   - Correlated subqueries

6. **Functions**

   - String functions (LENGTH, REVERSE, LOWER, UPPER, SUBSTR)
   - Date functions (TO_DATE, EXTRACT, YEAR)
   - Conversion functions (TO_CHAR)

7. **Null Handling**
   - IS NULL and IS NOT NULL conditions
   - Handling null values in calculations

---

_Last updated: 2025-06-14_  
_Created by: Abhra_

```

This markdown file includes:
1. A structured organization of all 50 questions across 5 sets
2. The SQL schema for reference
3. Well-formatted SQL code blocks for each question
4. Clear categorization of SQL concepts
5. A summary of SQL concepts covered in the material
6. Metadata at the end with the current date and username you provided

The layout is designed to be both visually appealing and easy to navigate for study purposes. Each question is clearly labeled with its ID number, question text, SQL solution, and category.
```
