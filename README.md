# Gang-gang
PL/SQL Window function query project (document)

This project contains the use of PL/SQL window function in an oracle database. It contains the dataset of employee and queries using function such as LAG (), LEAD (), RANK (), DENSE_RANK () and PARTITION BY () to analyze data effectively 
 

Table Structure
The project includes an Employees table with the following fields:
•	Employee_ID (Primary Key)
•	Name (VARCHAR2)
•	Department (VARCHAR2)
•	Salary (NUMBER)
•	Hire_Date (DATE)

Step 1: Create the Employees Table
This table stores employee records, including their department, salary, and hire date.

```
CREATE TABLE Employees (
    Employee_ID NUMBER PRIMARY KEY,
    Name VARCHAR2(50),
    Department VARCHAR2(50),
    Salary NUMBER,
    Hire_Date DATE
);
```
Step 2: Insert Sample Data
This data represents a small set of employees working in different departments with varying salaries and hire dates.

```
INSERT INTO Employees VALUES (1, 'Alice', 'HR', 50000, TO_DATE('2020-01-15', 'YYYY-MM-DD'));
INSERT INTO Employees VALUES (2, 'Bob', 'HR', 60000, TO_DATE('2019-03-22', 'YYYY-MM-DD'));
INSERT INTO Employees VALUES (3, 'Charlie', 'IT', 70000, TO_DATE('2021-07-10', 'YYYY-MM-DD'));
INSERT INTO Employees VALUES (4, 'David', 'IT', 70000, TO_DATE('2020-05-18', 'YYYY-MM-DD'));
INSERT INTO Employees VALUES (5, 'Eve', 'IT', 80000, TO_DATE('2018-09-05', 'YYYY-MM-DD'));
INSERT INTO Employees VALUES (6, 'Frank', 'Finance', 90000, TO_DATE('2022-02-20', 'YYYY-MM-DD'));
INSERT INTO Employees VALUES (7, 'Grace', 'Finance', 85000, TO_DATE('2017-12-11', 'YYYY-MM-DD'));
INSERT INTO Employees VALUES (8, 'Hank', 'Finance', 92000, TO_DATE('2023-06-30', 'YYYY-MM-DD'));

COMMIT;
```
Queries and Explanations
1.	Compare Salary with Previous and Next Employee
This query uses LAG() and LEAD() to compare each employee’s salary with their previous and next records. The result helps identify trends in salary progression.

```
SELECT Employee_ID, Name, Department, Salary, 
       LAG(Salary) OVER (ORDER BY Employee_ID) AS Prev_Salary,
       LEAD(Salary) OVER (ORDER BY Employee_ID) AS Next_Salary,
       CASE
           WHEN Salary > LAG(Salary) OVER (ORDER BY Employee_ID) THEN 'HIGHER'
           WHEN Salary < LAG(Salary) OVER (ORDER BY Employee_ID) THEN 'LOWER'
           ELSE 'EQUAL'
       END AS Compared_to_Previous
FROM Employees;
``` 

Real-Life Application: This technique is useful in payroll analysis, where organizations track salary progression trends among employees.

2. Rank Employees by Salary within Department

This query ranks employees based on their salary within each department, showing the difference between RANK() and DENSE_RANK().

```
SELECT Employee_ID, Name, Department, Salary,
       RANK() OVER (PARTITION BY Department ORDER BY Salary DESC) AS Rank_Salary,
       DENSE_RANK() OVER (PARTITION BY Department ORDER BY Salary DESC) AS Dense_Rank_Salary
FROM Employees;
``` 
 
Real-Life Application: Used in performance reviews and compensation benchmarking to identify top earners within each department.

3. Get Top 3 Salaries per Department
This query retrieves the top 3 salaries per department while handling duplicate salaries properly.

```
SELECT * FROM (
    SELECT Employee_ID, Name, Department, Salary,
           RANK() OVER (PARTITION BY Department ORDER BY Salary DESC) AS Rank_Salary
    FROM Employees
) WHERE Rank_Salary <= 3;

 ```

Real-Life Application: Commonly used in HR analytics to reward the top performers within each department.

4. Get First 2 Hired Employees per Department
This query identifies the first two employees hired in each department using RANK().

```
SELECT * FROM (
    SELECT Employee_ID, Name, Department, Hire_Date,
           RANK() OVER (PARTITION BY Department ORDER BY Hire_Date ASC) AS Hire_Rank
    FROM Employees
) WHERE Hire_Rank <= 2;
```
 
Real-Life Application: Useful in workforce planning to analyze early hiring patterns and retention trends.

5. Calculate Maximum Salary per Department and Overall
This query calculates the highest salary within each department and across all employees using MAX() with window functions.

```
SELECT Employee_ID, Name, Department, Salary,
       MAX(Salary) OVER (PARTITION BY Department) AS Max_Salary_Department,
       MAX(Salary) OVER () AS Max_Salary_Overall
FROM Employees;

```
Real-Life Application: Helps HR teams determine department-wise salary ceilings and overall salary benchmarks within the company.

Conclusion
This project provides a structured approach to utilizing window functions for analyzing employee data in an Oracle database. These techniques are helpful in workforce analytics, payroll structuring, and business intelligence applications.

