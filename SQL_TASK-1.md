# SQL_TASK-1

```SQL
create table Department (DeptID int Primary key,DeptName varchar(50));
```

```SQL
create table Employee (EmpID int,Name varchar(50),Email varchar(50),City varchar(50),Mobile int,JoiningDate Date,Salary Decimal(10,2),
DeptID int FOREIGN key REFERENCES Department(DeptID))
```

```SQL
insert into Department values(1,'Computer')
insert into Department values(2,'Mechanical')
insert into Department values(3,'Chemical')
```

```SQL
insert into Employee values(1,'Yash','yash@gmail.com','jamnagar','123456','2024-2-12',80000,1)
insert into Employee values(2,'jayesh','jayesh12@gmail.com','rajkot','456789','2024-4-15',70000,2)
insert into Employee values(3,'parth','parth12@gmail.com','jamnagar','12345678','2024-5-10',50000,3)
insert into Employee values(4,'Jay','jay12@gmail.com','jamnagar','123','2024-6-10',60000,3)
insert into Employee values(5,'Mohit', Null ,'jamnagar','123','2024-6-10',60000,2)
insert into Employee values(6,'Mudit','Mudit@gmail.com','jamnagar', Null ,'2024-6-10',60000,1)
insert into Employee values(7,'Mayur','Mayur@gmail.com','rajkot', Null ,'2024-6-10',50000,1)
```

## 1. List all Employees Which belongs to jamnagar
```SQL
SELECT * FROM Employee WHERE City = 'JAMNAGAR';
```

## 2. List all Employees who joined after 02 Descember, 2024 and belong to either Computer or Chemical
```SQL
SELECT * FROM Employee 
WHERE JoiningDate > '2024-02-12' 
  AND DeptID IN (SELECT DeptID FROM Department WHERE DeptName IN ('Computer', 'Chemical'));
```

```SQL
SELECT * FROM 
	Employee E 
		inner JOIN Department D ON E.EmpID = D.DeptID 
	WHERE 
		E.JoiningDate > '2024-02-12' AND D.DeptName IN ('Computer','Chemical');
```

## 3. List all Employees with department name who don't have either mobile or email
```SQL
SELECT e.*, d.DeptName FROM Employee e 
Inner JOIN Department d ON e.DeptID = d.DeptID 
WHERE e.Mobile IS NULL OR e.Email IS NULL;
```

## 4. List top 5 employees as per salaries
```SQL
SELECT Top(5) * FROM Employee ORDER BY Salary DESC;
```

## 5. List top 3 employees department-wise as per salaries
```SQL
SELECT Top(3) E.* FROM Employee E 
INNER JOIN Department D ON E.EmpID = D.DeptID 
ORDER BY D.DeptName , E.Salary DESC
```

## 6. List City with Employee Count
```SQL
SElECT City , COUNT(EmpID) AS Employee_Count FROM Employee 
GROUP BY City
```

## 7. List City Wise Maximum, Minimum & Average Salaries
```SQL
SELECT City , 
MAX(Salary) AS max_sal , 
MIN(Salary) AS min_sal,
AVG(Salary) AS avg_sal 
FROM Employee GROUP BY City
```

## 8. List Department-wise City-wise Employee Count
```SQL
SELECT D.DeptName, E.City, COUNT(*) as EmployeeCount
FROM Employee E INNER JOIN Department D ON E.EmpID = D.DeptID
GROUP BY D.DeptName, E.City
```

```SQL
SELECT DeptName, City, COUNT(*) as EmployeeCount
FROM (
    SELECT e.DeptID, e.City, d.DeptName
    FROM Employee e
    INNER JOIN Department d ON e.DeptID = d.DeptID
) AS DeptCityInfo
GROUP BY DeptName, City;
```

## 9. List Departments with more than 2 employees
```SQL
SELECT d.DeptName 
FROM Employee e 
JOIN Department d ON e.DeptID = d.DeptID 
GROUP BY d.DeptName 
HAVING COUNT(e.EmpID) > 2;
```

## 10. Give 10% increment in salary to all employees who belong to Mechanical Department
```SQL
UPDATE Employee 
SET Salary = Salary * 1.10 
WHERE DeptID = (SELECT DeptID FROM Department WHERE DeptName = 'Mechanical');
```

```SQL
UPDATE e
SET e.Salary = e.Salary * 1.10
FROM Employee e
INNER JOIN Department d ON e.DeptID = d.DeptID
WHERE d.DeptName = 'Mechanical';
```

## 11. Update City of Yash from Mumbai to Pune having 7 as Employee ID
```SQL
UPDATE Employee 
SET City = 'Pune' 
WHERE EmpID = 7 AND Name = 'Mayur';
```

## 12. Delete all the employees who belong to HR Department & Salary is more than 45,000
```SQL
DELETE FROM Employee 
WHERE DeptID = (SELECT DeptID FROM Department WHERE DeptName = 'HR') 
  AND Salary > 45000;
```

```SQL
DELETE e
FROM Employee e
INNER JOIN Department d ON e.DeptID = d.DeptID
WHERE d.DeptName = 'HR'
  AND e.Salary > 45000;
```

## 13. List Employees with same name with occurrence of name
```SQL
SELECT Name, COUNT(*) as Occurrence 
FROM Employee 
GROUP BY Name 
HAVING COUNT(*) > 1;
```

## 14. List Department-wise Average Salary
```SQL
SELECT d.DeptName, AVG(e.Salary) as AvgSalary 
FROM Employee e 
INNER JOIN Department d ON e.DeptID = d.DeptID 
GROUP BY d.DeptName;
```

## 15. List City wise highest paid employee
```SQL
SELECT City, Name, Salary 
FROM Employee e1 
WHERE Salary = (SELECT MAX(Salary) 
FROM Employee e2 WHERE e1.City = e2.City);
```
