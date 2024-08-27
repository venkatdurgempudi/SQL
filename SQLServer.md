### *SQL Server Data Types*

#### *1. Numeric Data Types*
- **`int`**: Integer data from -2,147,483,648 to 2,147,483,647.
  - Example: `DECLARE @age int = 25;`
- **`smallint`**: Integer data from -32,768 to 32,767.
  - Example: `DECLARE @smallNumber smallint = 32000;`
- **`tinyint`**: Integer data from 0 to 255.
  - Example: `DECLARE @tinyNumber tinyint = 200;`
- **`bigint`**: Integer data from -2^63 to 2^63-1.
  - Example: `DECLARE @largeNumber bigint = 9000000000;`
- **`decimal(p,s)`**: Fixed precision and scale numbers. `p` is precision, `s` is scale.
  - Example: `DECLARE @price decimal(10,2) = 12345.67;`
- **`numeric(p,s)`**: Same as `decimal`.
  - Example: `DECLARE @rate numeric(5,3) = 12.345;`
- **`float`**: Approximate number data type with floating precision.
  - Example: `DECLARE @floatNumber float = 123.4567;`
- **`real`**: Same as `float`, but with less precision.
  - Example: `DECLARE @realNumber real = 123.45;`

#### *2. Character String Data Types*
- **`char(n)`**: Fixed-length non-Unicode string. `n` specifies the string length.
  - Example: `DECLARE @code char(5) = 'A123';`
- **`varchar(n)`**: Variable-length non-Unicode string. `n` specifies the max string length.
  - Example: `DECLARE @name varchar(50) = 'John Doe';`
- **`text`**: Variable-length non-Unicode string up to 2 GB.
  - Example: `DECLARE @description text = 'This is a long description...';`

#### *3. Unicode Character String Data Types*
- **`nchar(n)`**: Fixed-length Unicode string.
  - Example: `DECLARE @unicodeCode nchar(10) = N'AB123';`
- **`nvarchar(n)`**: Variable-length Unicode string.
  - Example: `DECLARE @unicodeName nvarchar(100) = N'John Doe';`
- **`ntext`**: Variable-length Unicode string up to 2 GB.
  - Example: `DECLARE @unicodeDescription ntext = N'This is a long Unicode description...';`

#### *4. Date and Time Data Types*
- **`date`**: Stores date only (YYYY-MM-DD).
  - Example: `DECLARE @birthDate date = '1980-05-15';`
- **`time`**: Stores time only (HH:MM:SS).
  - Example: `DECLARE @meetingTime time = '14:30:00';`
- **`datetime`**: Stores date and time (YYYY-MM-DD HH:MM:SS).
  - Example: `DECLARE @created datetime = '2024-08-26 10:45:00';`
- **`smalldatetime`**: Stores date and time with less precision.
  - Example: `DECLARE @logTime smalldatetime = '2024-08-26 10:00';`
- **`datetime2`**: Stores date and time with higher precision.
  - Example: `DECLARE @timestamp datetime2 = '2024-08-26 10:45:00.1234567';`
- **`datetimeoffset`**: Stores date and time with time zone awareness.
  - Example: `DECLARE @eventTime datetimeoffset = '2024-08-26 10:45:00 -07:00';`

#### *5. Binary Data Types*
- **`binary(n)`**: Fixed-length binary data.
  - Example: `DECLARE @binData binary(5) = 0x12345;`
- **`varbinary(n)`**: Variable-length binary data.
  - Example: `DECLARE @varBinData varbinary(50) = 0x1234567890ABCDEF;`
- **`image`**: Variable-length binary data up to 2 GB.
  - Example: `DECLARE @imageData image = 0x1234567890ABCDEF...;`

#### *6. Other Data Types*
- **`bit`**: Integer with a value of 0, 1, or NULL.
  - Example: `DECLARE @isActive bit = 1;`
- **`uniqueidentifier`**: Stores a globally unique identifier (GUID).
  - Example: `DECLARE @guid uniqueidentifier = NEWID();`
- **`xml`**: Stores XML data.
  - Example: `DECLARE @xmlData xml = '<root><element>Value</element></root>';`
- **`sql_variant`**: Stores values of various data types.
  - Example: `DECLARE @data sql_variant = 123;`

### *SQL Server Constraints*

#### *1. `PRIMARY KEY`*
Ensures that the column(s) uniquely identify each row in the table.
- Example:
  sql
  CREATE TABLE Employees (
      EmployeeID int PRIMARY KEY,
      Name varchar(100)
  );
  

#### *2. `FOREIGN KEY`*
Establishes a relationship between two tables.
- Example:
  sql
  CREATE TABLE Orders (
      OrderID int PRIMARY KEY,
      EmployeeID int,
      FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
  );
  

#### *3. `UNIQUE`*
Ensures that all values in a column are unique.
- Example:
  sql
  CREATE TABLE Users (
      UserID int PRIMARY KEY,
      Email varchar(100) UNIQUE
  );
  

#### *4. `NOT NULL`*
Ensures that a column cannot have a NULL value.
- Example:
  sql
  CREATE TABLE Products (
      ProductID int PRIMARY KEY,
      Name varchar(100) NOT NULL
  );
  

#### *5. `CHECK`*
Ensures that all values in a column satisfy a specific condition.
- Example:
  sql
  CREATE TABLE Employees (
      EmployeeID int PRIMARY KEY,
      Age int CHECK (Age >= 18)
  );
  

#### *6. `DEFAULT`*
Sets a default value for a column if none is provided.
- Example:
  sql
  CREATE TABLE Accounts (
      AccountID int PRIMARY KEY,
      CreatedDate datetime DEFAULT GETDATE()
  );
  

### *Summary Example*
Combining data types and constraints:
sql
CREATE TABLE Customers (
    CustomerID int PRIMARY KEY,
    FirstName varchar(50) NOT NULL,
    LastName varchar(50) NOT NULL,
    Email varchar(100) UNIQUE,
    BirthDate date CHECK (BirthDate > '1900-01-01'),
    CreatedDate datetime DEFAULT GETDATE()
);


This example creates a `Customers` table with various data types and constraints applied to the columns.

Let me know if you need more details or examples!
