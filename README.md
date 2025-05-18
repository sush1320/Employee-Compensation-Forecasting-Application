Employee Compensation Forecasting Application

This is a proof-of-concept web application designed for HR/business stakeholders to analyze, forecast, and export employee compensation data.

Features

Filter active/inactive employees by role and location

View average compensation by location

Display bar chart comparing location-wise compensation

Group employees by experience ranges

Simulate fixed and custom compensation increments

Export filtered employee data to CSV



---

Tech Stack

Frontend: React, Chart.js

Backend: ASP.NET Core Web API (C#)

Database: SQL Server



---

Prerequisites

.NET SDK 6.0+

SQL Server

Node.js + npm



---

Setup Guide


1. SQL Server Setup

a. Create Database and Tables

Use SQL Server Management Studio (SSMS) or another SQL client.

CREATE DATABASE EmployeeDB;
GO

USE EmployeeDB;

CREATE TABLE Role (
RoleID INT PRIMARY KEY IDENTITY,
RoleName NVARCHAR(100) NOT NULL
);

CREATE TABLE Location (
LocationID INT PRIMARY KEY IDENTITY,
LocationName NVARCHAR(100) NOT NULL
);

CREATE TABLE Employee (
EmployeeID INT PRIMARY KEY IDENTITY,
Name NVARCHAR(100),
RoleID INT FOREIGN KEY REFERENCES Role(RoleID),
LocationID INT FOREIGN KEY REFERENCES Location(LocationID),
ExperienceYears INT,
Compensation DECIMAL(18, 2),
Status NVARCHAR(10)
);

b. Insert Sample Data

INSERT INTO Role (RoleName) VALUES ('Engineer'), ('Manager'), ('Analyst');
INSERT INTO Location (LocationName) VALUES ('New York'), ('London'), ('Bangalore');

INSERT INTO Employee (Name, RoleID, LocationID, ExperienceYears, Compensation, Status)
VALUES
('Alice', 1, 1, 3, 60000, 'Active'),
('Bob', 2, 2, 5, 80000, 'Active'),
('Charlie', 1, 3, 2, 55000, 'Inactive');

c. Stored Procedures

CREATE PROCEDURE GetEmployees
@RoleID INT = NULL,
@LocationID INT = NULL,
@IncludeInactive BIT = 0
AS
BEGIN
SELECT *
FROM Employee E
JOIN Role R ON E.RoleID = R.RoleID
JOIN Location L ON E.LocationID = L.LocationID
WHERE (@RoleID IS NULL OR E.RoleID = @RoleID)
AND (@LocationID IS NULL OR E.LocationID = @LocationID)
AND (@IncludeInactive = 1 OR E.Status = 'Active');
END;


---

2. Backend Setup (ASP.NET Core)

Navigate to backend folder:


cd backend

Open appsettings.json and update your SQL Server connection string:


"ConnectionStrings": {
"DefaultConnection": "Server=YOUR_SERVER;Database=EmployeeDB;Trusted_Connection=True;"
}

Build and run the API:


dotnet run

API will be available at http://localhost:5000/api/employee



---

3. Frontend Setup (React)

Open a new terminal:


cd ../frontend

Install dependencies:


npm install

Start the React app:


npm start

App will open at http://localhost:3000



---

User Stories Fulfilled

1. Filter and Display Active Employees by Role

Dropdowns for role/location

Toggle for including inactive

Bar chart for average compensation by location


2. Group Employees by Years of Experience

Select grouping by location or role

Bar chart shows counts


3. Simulate Compensation Increments

Input global % increment

Bonus: Custom % per employee/location


4. Download Filtered Employee Data

Export filtered data to CSV

Includes all relevant fields and applied increments



---

