# SQL-
SQL Scripts for Data Retrieval and Analysis
Three tables representing XYZ Company and performing queries for answering basic business questions. 
--Table 1
CREATE TABLE EmployeeDemographics
(EmployeeID int, 
FirstName varchar(50),
LastName varchar(50),
Age int,
Gender varchar(50)
)
--Table 2 
CREATE TABLE EmployeeSalary
(EmployeeID int, 
JobTitle varchar(50),
Salary int)

--INSERT SAMPLE DATA 
INSERT INTO EmployeeDemographics VALUES 
(1001, 'Jim', 'Halpert', 30, 'Male')
INSERT INTO EmployeeSalary VALUES 
(1001, 'Salesman', 45000)

Insert into EmployeeDemographics VALUES
(1002, 'Pam', 'Beasley', 30, 'Female'),
(1003, 'Dwight', 'Schrute', 29, 'Male'),
(1004, 'Angela', 'Martin', 31, 'Female'),
(1005, 'Toby', 'Flenderson', 32, 'Male'),
(1006, 'Michael', 'Scott', 35, 'Male'),
(1007, 'Meredith', 'Palmer', 32, 'Female'),
(1008, 'Stanley', 'Hudson', 38, 'Male'),
(1009, 'Kevin', 'Malone', 31, 'Male')

Insert Into EmployeeSalary VALUES
(1002, 'Receptionist', 36000),
(1003, 'Salesman', 63000),
(1004, 'Accountant', 47000),
(1005, 'HR', 50000),
(1006, 'Regional Manager', 65000),
(1007, 'Supplier Relations', 41000),
(1008, 'Salesman', 48000),
(1009, 'Accountant', 42000)

Insert into EmployeeDemographics VALUES
(1011, 'Ryan', 'Howard', 26, 'Male'),
(NULL, 'Holly', 'Flax', NULL, NULL),
(1013, 'Darryl', 'Philbin', NULL, 'Male')

Create Table WareHouseEmployeeDemographics 
(EmployeeID int, 
FirstName varchar(50), 
LastName varchar(50), 
Age int, 
Gender varchar(50)
)

Insert into WareHouseEmployeeDemographics VALUES
(1013, 'Darryl', 'Philbin', NULL, 'Male'),
(1050, 'Roy', 'Anderson', 31, 'Male'),
(1051, 'Hidetoshi', 'Hasagawa', 40, 'Male'),
(1052, 'Val', 'Johnson', 31, 'Female')

--Preview 3 DBs
SELECT * 
FROM EmployeeDemographics
SELECT *
FROM EmployeeSalary
SELECT * 
FROM WareHouseEmployeeDemographics

--Perform JOINS to preview data 
--JOIN two tables to have employee salary data 
SELECT d.FirstName, d.LastName, s.JobTitle, s.Salary
FROM EmployeeSalary s
INNER JOIN EmployeeDemographics d ON s.EmployeeID = d.EmployeeID
ORDER BY s.Salary DESC

--JOB TITLE and SALARY VIEW
SELECT d.FirstName, d.LastName, s.JobTitle, s.Salary
FROM EmployeeSalary s
INNER JOIN EmployeeDemographics d ON s.EmployeeID = d.EmployeeID
WHERE s.JobTitle = 'Salesman'
ORDER BY s.Salary 

--AGE AND SALARY VIEW 
SELECT d.Age, s.Salary, s.JobTitle 
FROM EmployeeSalary s
INNER JOIN EmployeeDemographics d ON d.EmployeeID = s.EmployeeID
WHERE Salary >= 40000
ORDER BY Age DESC

--AGGREGARTION SUMMARIES
--Total salary per month and annual 
SELECT SUM(Salary) as Total_Wages_per_month, (SUM(Salary) * 12) as annual_wages
FROM EmployeeSalary
--average
SELECT AVG(Salary) as avg_salary 
FROM EmployeeSalary
--salary range 
SELECT MAX(Salary) as max_salary, MIN(Salary) as min_salary 
FROM EmployeeSalary

--gender distribution
SELECT d.Gender, s.JobTitle
FROM EmployeeDemographics d
INNER JOIN EmployeeSalary s ON d.EmployeeID = s.EmployeeID
ORDER BY d.Gender DESC

--what is avg salary for salesman position?
SELECT Jobtitle, AVG(Salary) AS avg_salary
FROM EmployeeDemographics
INNER JOIN EmployeeSalary 
	ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
WHERE Jobtitle = 'Salesman'
GROUP BY Jobtitle

--UNIONS combine two tables ensuring both have same structure in columns and data types 
SELECT *
FROM EmployeeDemographics
UNION 
SELECT *
FROM WareHouseEmployeeDemographics
ORDER BY EmployeeID DESC

--CASE Statements for Categorizing data  
SELECT FirstName, Lastname, Age,
CASE 
	WHEN Age> 30 THEN 'Old'
	WHEN Age BETWEEN 27 AND 30 THEN  'Young'
	ELSE 'Baby'
END
FROM EmployeeDemographics
WHERE Age is not null 
Order by Age 


--CASE FOR CALCULATING A BONUS PER JOBTITLE  
SELECT FirstName, LastName, JobTitle, Salary,
CASE 
	WHEN JobTitle= 'Salesman' THEN Salary + (Salary * .10)
	WHEN JobTitle= 'Accountant' THEN Salary + (Salary * .05)
	WHEN JobTitle= 'HR' THEN Salary + (Salary * 0.01)
	ELSE Salary + (Salary * .03)
END AS Salary_after_raise
FROM EmployeeDemographics
JOIN EmployeeSalary ON EmployeeDemographics.EmployeeID= EmployeeSalary.EmployeeID

--CASE TO REVIEW SALARY , AGE, GENDER 
SELECT Age, Gender, Salary,
CASE 
	WHEN Salary <= 40000 THEN 'lower range'
	WHEN Salary BETWEEN 50000 AND 70000 THEN 'high range'
	ELSE 'normal range'
END AS Salary_review
FROM EmployeeDemographics
INNER JOIN EmployeeSalary ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
ORDER BY 1,2,3 DESC

--CASE TO LOOK AT FEMALE POSITIONS, AGE, SALARY ONLY 
SELECT a.Age, b.Salary, a.Gender, b.JobTitle,
CASE 
	WHEN Salary <= 40000 THEN 'low salary'
	ELSE 'normal'
END
FROM EmployeeDemographics a
FULL JOIN EmployeeSalary b ON a.EmployeeID = b.EmployeeID
WHERE Gender = 'Female' 

--COUNTS PER JOBTITLE 
SELECT JobTitle, COUNT(JobTitle) as count_per_title
FROM EmployeeDemographics
JOIN EmployeeSalary ON EmployeeDemographics.EmployeeID= EmployeeSalary.EmployeeID
GROUP BY JobTitle
HAVING COUNT(JobTitle) > 1

--job title and avg salary 
SELECT JobTitle, AVG(Salary) as avg_salary
FROM EmployeeDemographics
JOIN EmployeeSalary ON EmployeeDemographics.EmployeeID= EmployeeSalary.EmployeeID
GROUP BY JobTitle
HAVING AVG(Salary) > 45000

--job title and avg age 
SELECT JobTitle, AVG(age) as age_avg
FROM EmployeeDemographics d
INNER JOIN EmployeeSalary s ON d.EmployeeID = s.EmployeeID
GROUP BY JobTitle 

--MODIFY A RECORD EXAMPLE 
UPDATE EmployeeDemographics
set EmployeeID= 1012 
WHERE FirstName= 'Holly' AND LastName = 'Flax'

UPDATE EmployeeDemographics
set Age = 31, Gender= 'Female'
WHERE FirstName= 'Holly' AND LastName = 'Flax'
--CHECK CHANGE MADE 
SELECT * 
FROM WareHouseEmployeeDemographics

--THREE TABLE JOIN EXAMPLE 
SELECT demo.EmployeeID, demo.FirstName, sal.JobTitle, ware.Age
FROM EmployeeDemographics demo
LEFT JOIN EmployeeSalary sal ON demo.EmployeeID= sal.EmployeeID
LEFT JOIN WareHouseEmployeeDemographics ware ON demo.EmployeeID = ware.EmployeeID

--COUNTS 
--Getting the counts per gender available in company 
SELECT FirstName, LastName, Gender, Salary,
	COUNT(Gender) OVER(PARTITION BY Gender) as Total_Gender_Count
FROM EmployeeDemographics 
JOIN EmployeeSalary
ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID

--Getting the counts per age in company 
SELECT FirstName, LastName, Age,
COUNT(Age) OVER(PARTITION BY Age) AS Total_Age_Count
FROM EmployeeDemographics
ORDER BY FirstName, LastName, Age

--Getting the counts per job title 
SELECT demo.LastName, demo.Gender, sal.JobTitle, 
	COUNT(JobTitle) OVER(PARTITION BY JobTitle)as count_job_title 
FROM EmployeeDemographics as demo 
JOIN EmployeeSalary as sal ON sal.EmployeeID = demo.EmployeeID
GROUP BY demo.LastName, demo.Gender, sal.JobTitle

--Getting counts per salary 
SELECT demo.LastName, sal.JobTitle, sal.Salary, COUNT(Salary) OVER(PARTITION BY Salary) AS count_salary_amt
FROM EmployeeSalary sal 
JOIN EmployeeDemographics demo ON demo.EmployeeID= sal.EmployeeID
GROUP BY demo.LastName, sal.JobTitle, sal.Salary 
