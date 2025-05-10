# Sample-MS-SQL-queries

DECLARE @XML XML;
SELECT @XML =CAST(BULKCOLUMN AS XML)
FROM  OPENROWSET (BULK 'C:\Users\vkhan\Downloads\large-dataset_Latest.xml', SINGLE_CLOB) AS C;
SELECT x.value ('(id)[1]','INT') AS EmployeeID,
x.value('(firstName)[1]', 'VARCHAR(100)') as EmployeeFirstName,
x.value('(lastName)[1]', 'VARCHAR(100)') as EmployeeLastName,
x.value('(position)[1]', 'VARCHAR(100)') as Position,
x.value('(salary)[1]', 'VARCHAR(100)') as Salary,
x.value('(hireDate)[1]', 'DATE') as Hiredate,
x.value('(department)[1]', 'VARCHAR(100)') as department,
p.value('(name)[1]','VARCHAR(100)') AS ProjectName,
p.value('(startDate)[1]','DATE') AS startdate,
p.value('(endDate)[1]','DATE') AS enddate
--INTO #TEMPTABLE1
FROM @XML.nodes('/employees/employee') as t(x)
CROSS APPLY x.nodes('projects') as s(y)
cross apply y.nodes('project') as h(p);
SELECT * INTO Employeeproject from #TEMPTABLE1;
select * from Employeeproject;

declare @xml XML;
SELECT @XML=CAST(BULKCOLUMN AS XML)
FROM OPENROWSET (BULK 'C:\Users\vkhan\Downloads\sample_CustomersOrders.xml', SINGLE_CLOB) AS X;
SELECT c.value('@CustomerID','VARCHAR(100)') as employee_id
FROM @XML.nodes('/Root/Customers/Customer') as t(c);

declare @xml XML;
SELECT @XML=CAST(BULKCOLUMN AS XML)
FROM OPENROWSET(BULK 'C:\Users\vkhan\Downloads\large-dataset_Latest.xml',
SINGLE_CLOB) AS C;
SELECT x.value('(id)[1]','INT') AS EmployeeID,
k.value('(name)[1]','VARCHAR(100)') as ProjectName
FROM @XML.nodes('/employees/employee') as t(x)
CROSS APPLY X.nodes('projects') as g(h)
CROSS APPLY h.nodes('project') as j(k);

CREATE TABLE OFFICE_EMPLOYEE(ID varchar(100) IDENTITY(1000,1) PRIMARY KEY,
NAME VARCHAR(100),
EMAIL NVARCHAR(100) MASKED WITH (FUNCTION='EMAIL()'),
SALARY INT MASKED WITH (FUNCTION='DEFAULT()'));

INSERT INTO OFFICE_EMPLOYEE VALUES('VIVEKANANDA KHAN','VKHAN45@GMAIL.COM','3900');
SELECT * FROM OFFICE_EMPLOYEE;
REVOKE UNMASK TO currentuser;
truncate table OFFICE_EMPLOYEE;

SELECT LEFT(T.NATIONALIDNUMBER,1) +'****'+ RIGHT(T.NATIONALIDNUMBER,1) FROM AdventureWorks2022.HumanResources.Employee t;

SELECT * FROM XML_EMPLOYEE_DATA
GO

CREATE PARTITION FUNCTION PF_ID(INT)
AS RANGE LEFT FOR VALUES (2,5,9);

CREATE PARTITION SCHEME PS_XMLDATA
AS PARTITION PF_ID ALL TO ([PRIMARY]);

CREATE TABLE XML_EMPLOYEE_DATA_NEW (ID INT,
NAME VARCHAR(100))
ON PS_XMLDATA(ID)

INSERT INTO XML_EMPLOYEE_DATA_NEW
SELECT * FROM XML_EMPLOYEE_DATA;
SELECT * FROM SYS.partition_schemes;
