# ADO.NET Basics

> *Click &#9733; if you like the project. Your contributions are heartily ♡ welcome.*

<br/>

## Table of Contents

* [ADO.NET Fundamentals](#-1-adonet-fundamentals)
* [Core ADO.NET Objects](#-2-core-adonet-objects)
* [Connection Handling & Lifecycle](#-3-connection-handling--lifecycle)
* [SqlCommand & Command Execution](#-4-sqlcommand--command-execution)
* [SqlDataReader — Connected Model](#-5-sqldatareader--connected-model)
* [DataSet & DataAdapter — Disconnected Model](#-6-dataset--dataadapter--disconnected-model)
* [Parameters & SQL Injection Prevention](#-7-parameters--sql-injection-prevention)
* [Stored Procedures in ADO.NET](#-8-stored-procedures-in-adonet)
* [Transactions in ADO.NET](#-9-transactions-in-adonet)
* [Exceptions & Error Handling](#-10-exceptions--error-handling)
* [Performance & Best Practices](#-11-performance--best-practices)
* [ADO.NET vs EF vs Dapper](#-12-adonet-vs-ef-vs-dapper)

<br/>

## # 1. ADO.NET FUNDAMENTALS

<br/>

## Q. What is ADO.NET? What is its role in the .NET framework?

**ADO.NET** (ActiveX Data Objects .NET) is the data access technology in the .NET framework that provides a set of classes for connecting to data sources, executing commands, and managing data in a disconnected or connected manner.

**Role in .NET framework:**

```
Application Layer
      ↓
ADO.NET Layer (System.Data)
      ↓
.NET Data Provider (SqlClient, OracleClient, OleDb, ODBC)
      ↓
Database (SQL Server, Oracle, MySQL, etc.)
```

**Key namespaces:**

| Namespace                          | Purpose                                    |
|------------------------------------|--------------------------------------------|
| `System.Data`                      | Core objects: `DataSet`, `DataTable`, `DataRow` |
| `System.Data.SqlClient`            | Legacy SQL Server provider (Framework)     |
| `Microsoft.Data.SqlClient`         | Modern SQL Server provider (recommended)   |
| `System.Data.OleDb`                | OLE DB provider (Access, Excel)            |
| `System.Data.Odbc`                 | ODBC provider (generic)                    |
| `System.Data.Common`               | Abstract base classes                      |

> **Note:** `Microsoft.Data.SqlClient` (NuGet package) is the current recommended SQL Server provider. It supports Always Encrypted, Active Directory authentication, and modern TLS. `System.Data.SqlClient` is legacy.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between Connected and Disconnected architecture in ADO.NET?

| Aspect              | Connected                              | Disconnected                               |
|---------------------|----------------------------------------|--------------------------------------------|
| Connection state    | Open throughout operation              | Open briefly (fill/update only)            |
| Key objects         | `SqlConnection`, `SqlCommand`, `SqlDataReader` | `SqlDataAdapter`, `DataSet`, `DataTable` |
| Data location       | Streamed from database                 | Cached in memory                           |
| Performance         | Faster for large reads (streaming)     | Better for offline/batch scenarios         |
| Scalability         | Holds connection resource              | Releases connection quickly                |
| Use case            | Real-time reporting, large result sets | Web forms, grids, offline editing          |
| Concurrency         | Managed by DB                          | Manual (optimistic by default)             |

```csharp
// Connected — connection stays open while reading
using var connection = new SqlConnection(connectionString);
connection.Open();
using var command = new SqlCommand("SELECT * FROM Products", connection);
using var reader  = command.ExecuteReader(); // connection must stay open
while (reader.Read())
    Console.WriteLine(reader["ProductName"]);
// connection closed here

// Disconnected — connection opened briefly to fill DataSet
using var connection = new SqlConnection(connectionString);
var adapter = new SqlDataAdapter("SELECT * FROM Products", connection);
var dataSet = new DataSet();
adapter.Fill(dataSet, "Products");  // connection opened/closed internally
// Work with dataSet.Tables["Products"] without any DB connection
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the Provider Model in ADO.NET?

ADO.NET uses a **provider model** where each database vendor supplies a .NET data provider that implements standard interfaces. This allows code written against abstractions to work with any database.

```csharp
// Abstract base classes / interfaces
System.Data.Common.DbConnection    // base of SqlConnection, OracleConnection
System.Data.Common.DbCommand       // base of SqlCommand, OracleCommand
System.Data.Common.DbDataReader    // base of SqlDataReader
System.Data.Common.DbDataAdapter   // base of SqlDataAdapter

// Provider-agnostic code using DbProviderFactory
using System.Data.Common;

DbProviderFactory factory = DbProviderFactories.GetFactory("Microsoft.Data.SqlClient");
using DbConnection conn   = factory.CreateConnection();
conn.ConnectionString     = connectionString;
conn.Open();

using DbCommand cmd = factory.CreateCommand();
cmd.Connection      = conn;
cmd.CommandText     = "SELECT COUNT(*) FROM Products";
int count           = (int)cmd.ExecuteScalar();
```

**Common providers:**

| Provider                    | NuGet Package                    | Target DB          |
|-----------------------------|----------------------------------|--------------------|
| `Microsoft.Data.SqlClient`  | `Microsoft.Data.SqlClient`       | SQL Server / Azure SQL |
| `Npgsql`                    | `Npgsql`                         | PostgreSQL         |
| `MySqlConnector`            | `MySqlConnector`                 | MySQL / MariaDB    |
| `Oracle.ManagedDataAccess`  | `Oracle.ManagedDataAccess.Core`  | Oracle DB          |
| `Microsoft.Data.Sqlite`     | `Microsoft.Data.Sqlite`          | SQLite             |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 2. CORE ADO.NET OBJECTS

<br/>

## Q. What are the core ADO.NET objects and their purposes?

| Object             | Namespace                   | Purpose                                                    |
|--------------------|-----------------------------|------------------------------------------------------------|
| `SqlConnection`    | Microsoft.Data.SqlClient    | Opens and manages a DB connection                          |
| `SqlCommand`       | Microsoft.Data.SqlClient    | Executes SQL text or stored procedures                     |
| `SqlDataReader`    | Microsoft.Data.SqlClient    | Forward-only, read-only, connected result set streaming    |
| `SqlDataAdapter`   | Microsoft.Data.SqlClient    | Bridge between DB and `DataSet` (fill/update)              |
| `SqlParameter`     | Microsoft.Data.SqlClient    | Parameterized query values, prevents SQL injection         |
| `SqlTransaction`   | Microsoft.Data.SqlClient    | Wraps multiple commands in an atomic unit                  |
| `DataSet`          | System.Data                 | In-memory relational cache (disconnected)                  |
| `DataTable`        | System.Data                 | Single in-memory table with rows and columns               |
| `DataRow`          | System.Data                 | A single row in a `DataTable`                              |
| `DataColumn`       | System.Data                 | Defines a column\'s schema in a `DataTable`                 |
| `DataRelation`     | System.Data                 | Parent-child relationship between `DataTable`s             |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which ADO.NET object keeps the connection open? Which works without a connection?

```csharp
// SqlDataReader — REQUIRES open connection the entire time
using var connection = new SqlConnection(connectionString);
connection.Open();
using var command = new SqlCommand("SELECT Id, Name FROM Customers", connection);
using var reader  = command.ExecuteReader();

// Connection MUST stay open while iterating reader
while (reader.Read())
{
    int id    = reader.GetInt32(0);
    string name = reader.GetString(1);
    Console.WriteLine($"{id}: {name}");
}
// reader.Dispose() → safe to close connection now

// DataSet — works WITHOUT active connection
using var adapter = new SqlDataAdapter("SELECT Id, Name FROM Customers", connectionString);
var ds = new DataSet();
adapter.Fill(ds, "Customers"); // connection opened/closed internally

// From here — no DB connection required
foreach (DataRow row in ds.Tables["Customers"]!.Rows)
    Console.WriteLine($"{row["Id"]}: {row["Name"]}");

// Modify in memory
DataRow newRow = ds.Tables["Customers"]!.NewRow();
newRow["Name"] = "Alice";
ds.Tables["Customers"]!.Rows.Add(newRow);

// Push changes back (adapter handles connection internally)
var commandBuilder = new SqlCommandBuilder(adapter);
adapter.Update(ds, "Customers");
```

**Rule:** `SqlDataReader` holds connection open. `DataSet`/`DataTable` are fully **disconnected** in-memory structures.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 3. CONNECTION HANDLING & LIFECYCLE

<br/>

## Q. How do you create, open, and close a SqlConnection properly?

```csharp
// Connection string formats
string connStr = "Server=localhost;Database=Northwind;Integrated Security=True;TrustServerCertificate=True;";

// SQL Server Authentication
string connStrSql = "Server=myserver;Database=mydb;User Id=sa;Password=P@ssw0rd;Encrypt=True;";

// Azure SQL
string connStrAzure = "Server=myserver.database.windows.net;Database=mydb;" +
                      "Authentication=Active Directory Default;Encrypt=True;";

// Approach 1: using statement (RECOMMENDED — guarantees Dispose/Close)
using (var connection = new SqlConnection(connStr))
{
    connection.Open();
    Console.WriteLine($"State: {connection.State}");        // Open
    Console.WriteLine($"Database: {connection.Database}");  // Northwind
    // ... execute commands
} // Dispose() called → connection returned to pool

// Approach 2: C# 8 using declaration
using var connection2 = new SqlConnection(connStr);
connection2.Open();
// ... execute commands
// Disposed at end of enclosing scope

// Approach 3: try/finally (explicit control)
var connection3 = new SqlConnection(connStr);
try
{
    connection3.Open();
    // ... execute commands
}
finally
{
    connection3.Close(); // or connection3.Dispose()
}

// Async open (preferred in ASP.NET Core)
using var connection4 = new SqlConnection(connStr);
await connection4.OpenAsync();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Connection Pooling in ADO.NET? How does it work?

**Connection pooling** reuses physical database connections instead of creating a new one for every request. ADO.NET enables pooling by default for `SqlConnection`.

```csharp
// How pooling works internally:
// 1. First Open() → creates physical connection, adds to pool
// 2. Subsequent Open() with same connection string → reuses pooled connection
// 3. Close()/Dispose() → returns connection to pool (NOT physically closed)
// 4. Pool grows up to Max Pool Size (default 100)

// Connection string pool settings
string connStr =
    "Server=localhost;Database=mydb;Integrated Security=True;" +
    "Min Pool Size=5;" +         // pre-create 5 connections
    "Max Pool Size=100;" +       // maximum pool size
    "Connection Timeout=30;" +   // seconds to wait for available connection
    "Pooling=True;";             // default: true

// IMPORTANT: Always dispose connections to return them to pool
for (int i = 0; i < 10; i++)
{
    using var conn = new SqlConnection(connStr);
    await conn.OpenAsync(); // reuses pooled connection
    // ... query
} // Dispose returns to pool — NOT physically closed

// What happens if connection is NOT closed?
// → Pool exhausts → new requests throw SqlException:
//   "Timeout expired. The timeout period elapsed prior to obtaining a connection from the pool."

// Clear pool manually (e.g., after connection string change)
SqlConnection.ClearPool(connection);         // clear specific pool
SqlConnection.ClearAllPools();               // clear all pools

// Check pool state via connection events
connection.StateChange += (sender, e) =>
    Console.WriteLine($"State changed: {e.OriginalState} → {e.CurrentState}");
```

**Key rules:**
- Always use `using` or explicitly call `Close()`/`Dispose()`
- Use the same connection string format to benefit from pool reuse
- Never hold connections longer than needed

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What happens if you do not close a SqlConnection?

```csharp
// PROBLEM — connection not closed/disposed
public List<Product> GetProducts_BAD()
{
    var conn = new SqlConnection(connectionString); // connection opened
    conn.Open();
    var cmd    = new SqlCommand("SELECT * FROM Products", conn);
    var reader = cmd.ExecuteReader();
    var result = new List<Product>();
    while (reader.Read())
        result.Add(new Product { Id = reader.GetInt32(0), Name = reader.GetString(1) });
    return result;
    // conn, cmd, reader NEVER disposed → connection leak!
    // Pool drains → TimeoutException after max connections reached
}

// SOLUTION — using statements at every level
public async Task<List<Product>> GetProductsAsync(CancellationToken ct = default)
{
    var result = new List<Product>();

    await using var conn   = new SqlConnection(connectionString);
    await conn.OpenAsync(ct);

    await using var cmd    = new SqlCommand("SELECT Id, Name, Price FROM Products", conn);
    await using var reader = await cmd.ExecuteReaderAsync(ct);

    while (await reader.ReadAsync(ct))
    {
        result.Add(new Product
        {
            Id    = reader.GetInt32(0),
            Name  = reader.GetString(1),
            Price = reader.GetDecimal(2)
        });
    }
    return result;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 4. SQLCOMMAND & COMMAND EXECUTION

<br/>

## Q. What are the types of SqlCommand and execution methods?

**CommandType:**

```csharp
using Microsoft.Data.SqlClient;

// 1. Text (default) — inline SQL
cmd.CommandType = CommandType.Text;
cmd.CommandText = "SELECT * FROM Products WHERE CategoryId = @CategoryId";

// 2. StoredProcedure — calls an SP
cmd.CommandType = CommandType.StoredProcedure;
cmd.CommandText = "usp_GetProductsByCategory";

// 3. TableDirect — retrieves entire table (OleDb only, not SqlClient)
cmd.CommandType = CommandType.TableDirect;
cmd.CommandText = "Products";
```

**Execution methods:**

| Method              | Returns                     | Use For                                     |
|---------------------|-----------------------------|---------------------------------------------|
| `ExecuteReader()`   | `SqlDataReader`             | SELECT returning multiple rows              |
| `ExecuteScalar()`   | `object` (first col, row 1) | Aggregate: COUNT, SUM, single value         |
| `ExecuteNonQuery()` | `int` (rows affected)       | INSERT, UPDATE, DELETE, DDL                 |
| `ExecuteXmlReader()`| `XmlReader`                 | SELECT ... FOR XML                          |

```csharp
using var conn = new SqlConnection(connectionString);
await conn.OpenAsync();

// ExecuteReader — multiple rows
using var cmd1   = new SqlCommand("SELECT Id, Name FROM Products", conn);
using var reader = await cmd1.ExecuteReaderAsync();
while (await reader.ReadAsync())
    Console.WriteLine($"{reader.GetInt32(0)}: {reader.GetString(1)}");

// ExecuteScalar — single value (first column of first row)
using var cmd2 = new SqlCommand("SELECT COUNT(*) FROM Products", conn);
int count      = (int)await cmd2.ExecuteScalarAsync(); // returns object, cast needed
Console.WriteLine($"Product count: {count}");

// ExecuteScalar for MAX
using var cmd3    = new SqlCommand("SELECT MAX(Price) FROM Products", conn);
object maxPrice   = await cmd3.ExecuteScalarAsync();
decimal price     = maxPrice == DBNull.Value ? 0m : (decimal)maxPrice;

// ExecuteNonQuery — INSERT/UPDATE/DELETE
using var cmd4    = new SqlCommand(
    "INSERT INTO Products (Name, Price) VALUES (@Name, @Price)", conn);
cmd4.Parameters.AddWithValue("@Name",  "Widget");
cmd4.Parameters.AddWithValue("@Price", 9.99m);
int rowsAffected  = await cmd4.ExecuteNonQueryAsync();
Console.WriteLine($"Rows inserted: {rowsAffected}"); // 1
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What does `ExecuteScalar()` return? When do you use it?

`ExecuteScalar()` executes the query and returns the **first column of the first row** of the result set. Additional rows/columns are ignored.

```csharp
// Use cases for ExecuteScalar
using var conn = new SqlConnection(connectionString);
await conn.OpenAsync();

// 1. COUNT
int productCount = (int)(await new SqlCommand("SELECT COUNT(*) FROM Products", conn).ExecuteScalarAsync())!;

// 2. SUM
decimal? revenue = (decimal?)(await new SqlCommand("SELECT SUM(Amount) FROM Orders WHERE Year=2024", conn).ExecuteScalarAsync());

// 3. Get newly inserted ID (SCOPE_IDENTITY)
using var insertCmd = new SqlCommand(
    "INSERT INTO Products (Name, Price) VALUES (@Name, @Price); SELECT SCOPE_IDENTITY();", conn);
insertCmd.Parameters.AddWithValue("@Name", "Gadget");
insertCmd.Parameters.AddWithValue("@Price", 49.99m);
int newId = Convert.ToInt32(await insertCmd.ExecuteScalarAsync());
Console.WriteLine($"Inserted with ID: {newId}");

// 4. Check existence
using var existsCmd = new SqlCommand(
    "SELECT CASE WHEN EXISTS(SELECT 1 FROM Products WHERE Id=@Id) THEN 1 ELSE 0 END", conn);
existsCmd.Parameters.AddWithValue("@Id", 42);
bool exists = (int)(await existsCmd.ExecuteScalarAsync())! == 1;

// 5. Null handling — ExecuteScalar returns DBNull if no rows
using var nullCmd = new SqlCommand("SELECT MAX(Score) FROM Scores WHERE UserId = 999", conn);
object result = (await nullCmd.ExecuteScalarAsync())!;
int? maxScore = result == DBNull.Value ? null : (int?)result;
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 5. SQLDATAREADER — CONNECTED MODEL

<br/>

## Q. What is SqlDataReader? What are its key characteristics?

`SqlDataReader` provides a **forward-only, read-only, connected** stream of rows from a SQL Server database. It is the fastest way to read data when you only need to iterate once.

```csharp
using var conn   = new SqlConnection(connectionString);
await conn.OpenAsync();

using var cmd    = new SqlCommand(
    "SELECT Id, Name, Price, IsActive, CreatedAt FROM Products WHERE CategoryId = @CatId", conn);
cmd.Parameters.AddWithValue("@CatId", 5);

using var reader = await cmd.ExecuteReaderAsync(CommandBehavior.CloseConnection);
// CommandBehavior.CloseConnection → closes connection when reader is closed

// Check if any rows returned
Console.WriteLine($"Has rows: {reader.HasRows}");

while (await reader.ReadAsync())
{
    // Access by column index (fastest)
    int     id        = reader.GetInt32(0);

    // Access by column name (readable)
    string  name      = reader.GetString(reader.GetOrdinal("Name"));

    // Null-safe access
    decimal price     = reader.IsDBNull(2) ? 0m : reader.GetDecimal(2);
    bool    isActive  = reader.GetBoolean(3);
    DateTime created  = reader.GetDateTime(4);

    Console.WriteLine($"{id}: {name} — ${price} (Active: {isActive})");
}

// Multiple result sets (using NextResult)
using var multiCmd = new SqlCommand(
    "SELECT * FROM Products; SELECT * FROM Categories;", conn);
using var multiReader = await multiCmd.ExecuteReaderAsync();

do
{
    while (await multiReader.ReadAsync())
        Console.WriteLine(multiReader[0]);
} while (await multiReader.NextResultAsync()); // advance to next result set
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the key properties and methods of SqlDataReader?

| Member             | Type / Returns   | Description                                      |
|--------------------|------------------|--------------------------------------------------|
| `HasRows`          | `bool`           | `true` if the result set contains rows           |
| `IsClosed`         | `bool`           | `true` if the reader is closed                   |
| `FieldCount`       | `int`            | Number of columns in the current row             |
| `RecordsAffected`  | `int`            | Rows affected (for non-SELECT commands)          |
| `Read()`           | `bool`           | Advances to next row; `false` when exhausted     |
| `NextResult()`     | `bool`           | Advances to next result set                      |
| `GetName(int i)`   | `string`         | Column name by index                             |
| `GetOrdinal(name)` | `int`            | Column index by name (cache this for performance)|
| `IsDBNull(int i)`  | `bool`           | Check for `NULL` before typed read               |
| `GetString(i)`     | `string`         | Read string (throws if null)                     |
| `GetInt32(i)`      | `int`            | Read int (throws if null)                        |
| `GetDecimal(i)`    | `decimal`        | Read decimal (throws if null)                    |

```csharp
// Performance tip: cache ordinals outside the loop
using var reader = await cmd.ExecuteReaderAsync();

int idOrdinal    = reader.GetOrdinal("Id");
int nameOrdinal  = reader.GetOrdinal("Name");
int priceOrdinal = reader.GetOrdinal("Price");

while (await reader.ReadAsync())
{
    int     id    = reader.GetInt32(idOrdinal);
    string  name  = reader.GetString(nameOrdinal);
    decimal price = reader.IsDBNull(priceOrdinal) ? 0m : reader.GetDecimal(priceOrdinal);
}

// Can we update the DB using DataReader? NO — it is read-only
// To update, use SqlCommand.ExecuteNonQuery() or SqlDataAdapter.Update()
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 6. DATASET & DATAADAPTER — DISCONNECTED MODEL

<br/>

## Q. What is a DataSet? What is a DataAdapter and how do they work together?

**`DataSet`** is an in-memory representation of a relational database — it can hold multiple `DataTable`s, `DataRelation`s, and constraints, all without any database connection.

**`SqlDataAdapter`** is the bridge — it fills a `DataSet` from the DB and pushes changes back.

```csharp
// Fill DataSet with multiple tables
using var conn    = new SqlConnection(connectionString);
var adapter       = new SqlDataAdapter();

adapter.SelectCommand = new SqlCommand(
    "SELECT * FROM Orders; SELECT * FROM Customers;", conn);

var ds = new DataSet("Northwind");
adapter.Fill(ds); // opens/closes connection internally

// Rename tables for clarity
ds.Tables[0].TableName = "Orders";
ds.Tables[1].TableName = "Customers";

// Define relation (parent-child)
ds.Relations.Add("CustomerOrders",
    parentColumn: ds.Tables["Customers"]!.Columns["CustomerId"]!,
    childColumn:  ds.Tables["Orders"]!.Columns["CustomerId"]!);

// Traverse related rows
foreach (DataRow customer in ds.Tables["Customers"]!.Rows)
{
    Console.WriteLine($"Customer: {customer["CompanyName"]}");
    foreach (DataRow order in customer.GetChildRows("CustomerOrders"))
        Console.WriteLine($"  Order #{order["OrderId"]}: {order["OrderDate"]}");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use DataAdapter to Fill and Update a DataSet?

```csharp
using var conn    = new SqlConnection(connectionString);
var adapter       = new SqlDataAdapter("SELECT Id, Name, Price, Stock FROM Products", conn);

// SqlCommandBuilder auto-generates INSERT/UPDATE/DELETE commands
var builder       = new SqlCommandBuilder(adapter);
// OR set them manually:
adapter.InsertCommand = new SqlCommand(
    "INSERT INTO Products (Name, Price, Stock) VALUES (@Name, @Price, @Stock); SELECT SCOPE_IDENTITY();", conn);
adapter.InsertCommand.Parameters.Add("@Name",  SqlDbType.NVarChar, 100, "Name");
adapter.InsertCommand.Parameters.Add("@Price", SqlDbType.Decimal,    0, "Price");
adapter.InsertCommand.Parameters.Add("@Stock", SqlDbType.Int,        0, "Stock");
adapter.InsertCommand.UpdatedRowSource = UpdateRowSource.FirstReturnedRecord;

// Fill
var ds = new DataSet();
adapter.Fill(ds, "Products");

// Modify in-memory (no DB connection)
DataTable products = ds.Tables["Products"]!;

// INSERT — add new row
DataRow newRow = products.NewRow();
newRow["Name"]  = "New Widget";
newRow["Price"] = 19.99m;
newRow["Stock"] = 100;
products.Rows.Add(newRow);

// UPDATE — modify existing row
DataRow first = products.Rows[0];
first["Price"] = (decimal)first["Price"] * 1.1m; // 10% price increase

// DELETE — mark row for deletion
DataRow last = products.Rows[products.Rows.Count - 1];
last.Delete();

// Inspect row states
foreach (DataRow row in products.Rows)
    Console.WriteLine($"{row["Name"]}: RowState = {row.RowState}");
    // Added, Modified, Deleted, Unchanged

// Push ALL changes back to DB in one operation
int rowsAffected = adapter.Update(ds, "Products");
Console.WriteLine($"Rows affected: {rowsAffected}");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you work with DataTable directly?

```csharp
// Create DataTable programmatically
var table = new DataTable("Employees");

// Define schema
table.Columns.Add("Id",         typeof(int))    .AutoIncrement = true;
table.Columns.Add("Name",       typeof(string));
table.Columns.Add("Department", typeof(string));
table.Columns.Add("Salary",     typeof(decimal));
table.Columns.Add("HireDate",   typeof(DateTime));

// Primary key
table.PrimaryKey = new[] { table.Columns["Id"]! };

// Add rows
table.Rows.Add(null, "Alice",   "Engineering", 95000m, DateTime.Parse("2020-01-15"));
table.Rows.Add(null, "Bob",     "Marketing",   75000m, DateTime.Parse("2021-06-01"));
table.Rows.Add(null, "Charlie", "Engineering", 85000m, DateTime.Parse("2019-03-20"));

// Query with LINQ
var engineers = table.AsEnumerable()
    .Where(r => r.Field<string>("Department") == "Engineering")
    .OrderByDescending(r => r.Field<decimal>("Salary"))
    .Select(r => new { Name = r.Field<string>("Name"), Salary = r.Field<decimal>("Salary") });

foreach (var e in engineers)
    Console.WriteLine($"{e.Name}: ${e.Salary:N0}");

// DataView for filtering and sorting (bound to UI controls)
var view       = new DataView(table);
view.RowFilter = "Department = 'Engineering'";
view.Sort      = "Salary DESC";

foreach (DataRowView row in view)
    Console.WriteLine(row["Name"]);

// Compute aggregate
object total = table.Compute("SUM(Salary)", "Department = 'Engineering'");
Console.WriteLine($"Engineering total salary: ${total:N0}");

// Copy / Clone
DataTable clone = table.Clone();  // schema only, no rows
DataTable copy  = table.Copy();   // schema + rows
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 7. PARAMETERS & SQL INJECTION PREVENTION

<br/>

## Q. What is SQL Injection and how do you prevent it using SqlParameter?

**SQL Injection** occurs when user input is concatenated directly into a SQL string, allowing attackers to manipulate the query.

```csharp
// VULNERABLE — SQL Injection
string userInput = "'; DROP TABLE Users; --";
string sql = $"SELECT * FROM Users WHERE Username = '{userInput}'";
// Executes: SELECT * FROM Users WHERE Username = ''; DROP TABLE Users; --'

// BAD — string concatenation
public async Task<User?> GetUserBad(string username)
{
    using var conn = new SqlConnection(connectionString);
    await conn.OpenAsync();
    // DO NOT DO THIS:
    using var cmd = new SqlCommand(
        $"SELECT * FROM Users WHERE Username = '{username}'", conn);
    // ...
    return null;
}

// GOOD — parameterized query (ALWAYS use this)
public async Task<User?> GetUserSafe(string username)
{
    using var conn = new SqlConnection(connectionString);
    await conn.OpenAsync();

    using var cmd = new SqlCommand(
        "SELECT Id, Username, Email FROM Users WHERE Username = @Username", conn);

    // Method 1: AddWithValue (convenient but infers type)
    cmd.Parameters.AddWithValue("@Username", username);

    // Method 2: Explicit type and size (RECOMMENDED — avoids implicit conversion)
    cmd.Parameters.Add("@Username", SqlDbType.NVarChar, 100).Value = username;

    // Method 3: SqlParameter object
    cmd.Parameters.Add(new SqlParameter
    {
        ParameterName = "@Username",
        SqlDbType     = SqlDbType.NVarChar,
        Size          = 100,
        Value         = username
    });

    using var reader = await cmd.ExecuteReaderAsync();
    if (await reader.ReadAsync())
        return new User { Id = reader.GetInt32(0), Username = reader.GetString(1) };

    return null;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `AddWithValue` and `Add` for parameters?

```csharp
using var cmd = new SqlCommand("SELECT * FROM Orders WHERE CustomerId = @Id AND Status = @Status", conn);

// AddWithValue — infers SQL type from .NET type (may cause performance issues)
cmd.Parameters.AddWithValue("@Id",     customerId);  // infers Int32
cmd.Parameters.AddWithValue("@Status", "Pending");   // infers NVarChar(7) — bad! causes plan cache pollution

// Add with explicit type — RECOMMENDED
cmd.Parameters.Add("@Id",     SqlDbType.Int)             .Value = customerId;
cmd.Parameters.Add("@Status", SqlDbType.NVarChar, 50)    .Value = "Pending";

// Why explicit type matters:
// "Pending" via AddWithValue → NVarChar(7) — different plan for each string length
// "Pending" via Add NVarChar(50) → consistent, reused execution plan

// Handling NULL parameters
cmd.Parameters.Add("@Notes", SqlDbType.NVarChar, 500).Value =
    string.IsNullOrEmpty(notes) ? DBNull.Value : (object)notes;

// Alternative null handling
cmd.Parameters.Add("@Notes", SqlDbType.NVarChar, 500).Value = (object?)notes ?? DBNull.Value;
```

**Rule:** Use `Add` with explicit `SqlDbType` for production code. Reserve `AddWithValue` for quick scripts.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 8. STORED PROCEDURES IN ADO.NET

<br/>

## Q. How do you call a stored procedure using SqlCommand?

```sql
-- Sample stored procedure
CREATE PROCEDURE usp_GetOrdersByCustomer
    @CustomerId INT,
    @Status     NVARCHAR(50) = 'All'
AS
BEGIN
    SELECT OrderId, OrderDate, TotalAmount, Status
    FROM Orders
    WHERE CustomerId = @CustomerId
      AND (@Status = 'All' OR Status = @Status)
    ORDER BY OrderDate DESC;
END
```

```csharp
public async Task<List<Order>> GetOrdersByCustomerAsync(int customerId, string status = "All")
{
    var orders = new List<Order>();

    using var conn = new SqlConnection(connectionString);
    await conn.OpenAsync();

    using var cmd = new SqlCommand("usp_GetOrdersByCustomer", conn)
    {
        CommandType    = CommandType.StoredProcedure,
        CommandTimeout = 30 // seconds
    };

    cmd.Parameters.Add("@CustomerId", SqlDbType.Int)          .Value = customerId;
    cmd.Parameters.Add("@Status",     SqlDbType.NVarChar, 50) .Value = status;

    using var reader = await cmd.ExecuteReaderAsync();
    while (await reader.ReadAsync())
    {
        orders.Add(new Order
        {
            OrderId     = reader.GetInt32(0),
            OrderDate   = reader.GetDateTime(1),
            TotalAmount = reader.GetDecimal(2),
            Status      = reader.GetString(3)
        });
    }
    return orders;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use OUTPUT parameters and return values with stored procedures?

```sql
-- SP with INPUT, OUTPUT parameters and RETURN value
CREATE PROCEDURE usp_CreateOrder
    @CustomerId   INT,
    @TotalAmount  DECIMAL(18,2),
    @OrderId      INT OUTPUT,       -- output parameter
    @OrderNumber  NVARCHAR(20) OUTPUT
AS
BEGIN
    INSERT INTO Orders (CustomerId, TotalAmount, OrderDate, Status)
    VALUES (@CustomerId, @TotalAmount, GETUTCDATE(), 'Pending');

    SET @OrderId     = SCOPE_IDENTITY();
    SET @OrderNumber = 'ORD-' + RIGHT('0000' + CAST(@OrderId AS VARCHAR), 4);

    RETURN 0; -- 0 = success, non-zero = error code
END
```

```csharp
public async Task<(int OrderId, string OrderNumber, int ReturnCode)> CreateOrderAsync(
    int customerId, decimal totalAmount)
{
    using var conn = new SqlConnection(connectionString);
    await conn.OpenAsync();

    using var cmd = new SqlCommand("usp_CreateOrder", conn)
    {
        CommandType = CommandType.StoredProcedure
    };

    // INPUT parameters
    cmd.Parameters.Add("@CustomerId",  SqlDbType.Int)         .Value = customerId;
    cmd.Parameters.Add("@TotalAmount", SqlDbType.Decimal)     .Value = totalAmount;

    // OUTPUT parameters
    var orderIdParam = cmd.Parameters.Add("@OrderId", SqlDbType.Int);
    orderIdParam.Direction = ParameterDirection.Output;

    var orderNumberParam = cmd.Parameters.Add("@OrderNumber", SqlDbType.NVarChar, 20);
    orderNumberParam.Direction = ParameterDirection.Output;

    // RETURN value parameter — always named "@RETURN_VALUE"
    var returnParam = cmd.Parameters.Add("@RETURN_VALUE", SqlDbType.Int);
    returnParam.Direction = ParameterDirection.ReturnValue;

    await cmd.ExecuteNonQueryAsync();

    // Read output parameters AFTER execution
    int    orderId     = (int)orderIdParam.Value;
    string orderNumber = (string)orderNumberParam.Value;
    int    returnCode  = (int)returnParam.Value;

    return (orderId, orderNumber, returnCode);
}

// Usage
var (orderId, orderNumber, code) = await CreateOrderAsync(customerId: 7, totalAmount: 149.99m);
Console.WriteLine($"Created order #{orderId} ({orderNumber}) — return code: {code}");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you pass a table-valued parameter to a stored procedure?

```sql
-- Create user-defined table type
CREATE TYPE OrderItemType AS TABLE
(
    ProductId INT,
    Quantity  INT,
    UnitPrice DECIMAL(18, 2)
);

-- Stored procedure accepting table-valued parameter
CREATE PROCEDURE usp_BulkInsertOrderItems
    @OrderId    INT,
    @Items      OrderItemType READONLY
AS
BEGIN
    INSERT INTO OrderItems (OrderId, ProductId, Quantity, UnitPrice)
    SELECT @OrderId, ProductId, Quantity, UnitPrice FROM @Items;
END
```

```csharp
public async Task BulkInsertOrderItemsAsync(int orderId, List<OrderItem> items)
{
    // Build DataTable matching the TVP schema
    var tvp = new DataTable();
    tvp.Columns.Add("ProductId", typeof(int));
    tvp.Columns.Add("Quantity",  typeof(int));
    tvp.Columns.Add("UnitPrice", typeof(decimal));

    foreach (var item in items)
        tvp.Rows.Add(item.ProductId, item.Quantity, item.UnitPrice);

    using var conn = new SqlConnection(connectionString);
    await conn.OpenAsync();

    using var cmd = new SqlCommand("usp_BulkInsertOrderItems", conn)
    {
        CommandType = CommandType.StoredProcedure
    };

    cmd.Parameters.Add("@OrderId", SqlDbType.Int).Value = orderId;

    // Table-Valued Parameter
    var tvpParam = cmd.Parameters.AddWithValue("@Items", tvp);
    tvpParam.SqlDbType  = SqlDbType.Structured;
    tvpParam.TypeName   = "dbo.OrderItemType"; // must match DB type name

    await cmd.ExecuteNonQueryAsync();
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 9. TRANSACTIONS IN ADO.NET

<br/>

## Q. What is a SqlTransaction? How do you implement atomic operations?

A **SqlTransaction** groups multiple commands into a single **atomic** unit — either all succeed (commit) or all fail and are undone (rollback).

```csharp
// Use case: Bank transfer — debit one account, credit another
public async Task TransferFundsAsync(int fromAccountId, int toAccountId, decimal amount)
{
    using var conn = new SqlConnection(connectionString);
    await conn.OpenAsync();

    using var transaction = conn.BeginTransaction(
        IsolationLevel.ReadCommitted); // specify isolation level

    try
    {
        // Command 1: Debit source account
        using var debitCmd = new SqlCommand(
            "UPDATE Accounts SET Balance = Balance - @Amount WHERE AccountId = @AccountId AND Balance >= @Amount",
            conn, transaction);
        debitCmd.Parameters.Add("@Amount",    SqlDbType.Decimal).Value = amount;
        debitCmd.Parameters.Add("@AccountId", SqlDbType.Int)   .Value = fromAccountId;

        int debitRows = await debitCmd.ExecuteNonQueryAsync();
        if (debitRows == 0)
            throw new InvalidOperationException("Insufficient funds or account not found");

        // Command 2: Credit destination account
        using var creditCmd = new SqlCommand(
            "UPDATE Accounts SET Balance = Balance + @Amount WHERE AccountId = @AccountId",
            conn, transaction);
        creditCmd.Parameters.Add("@Amount",    SqlDbType.Decimal).Value = amount;
        creditCmd.Parameters.Add("@AccountId", SqlDbType.Int)   .Value = toAccountId;

        int creditRows = await creditCmd.ExecuteNonQueryAsync();
        if (creditRows == 0)
            throw new InvalidOperationException("Destination account not found");

        // Command 3: Log the transfer
        using var logCmd = new SqlCommand(
            "INSERT INTO TransferLog (FromAccount, ToAccount, Amount, TransferDate) VALUES (@From, @To, @Amt, GETUTCDATE())",
            conn, transaction);
        logCmd.Parameters.Add("@From", SqlDbType.Int)    .Value = fromAccountId;
        logCmd.Parameters.Add("@To",   SqlDbType.Int)    .Value = toAccountId;
        logCmd.Parameters.Add("@Amt",  SqlDbType.Decimal).Value = amount;
        await logCmd.ExecuteNonQueryAsync();

        // All commands succeeded — commit
        await transaction.CommitAsync();
        Console.WriteLine($"Transfer of ${amount} completed successfully");
    }
    catch
    {
        // Any failure — rollback ALL changes
        await transaction.RollbackAsync();
        Console.WriteLine("Transfer failed — all changes rolled back");
        throw; // re-throw to propagate
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Transaction Isolation Levels in ADO.NET?

```csharp
// Isolation levels control what a transaction can "see" from concurrent transactions
var levels = new Dictionary<IsolationLevel, string>
{
    [IsolationLevel.ReadUncommitted] = "Fastest — can read dirty (uncommitted) data",
    [IsolationLevel.ReadCommitted]   = "Default — reads only committed data, non-repeatable reads possible",
    [IsolationLevel.RepeatableRead]  = "Same row always returns same data within transaction",
    [IsolationLevel.Serializable]    = "Strictest — fully isolated, no phantom reads",
    [IsolationLevel.Snapshot]        = "Uses row versioning — high concurrency without read locks"
};

// Concurrency problems by isolation level:
// Dirty Read    | ReadUncommitted | possible
// NonRepeatable | ReadUncommitted, ReadCommitted | possible
// Phantom Read  | ReadUncommitted, ReadCommitted, RepeatableRead | possible

using var conn = new SqlConnection(connectionString);
await conn.OpenAsync();

// Set isolation level via BeginTransaction
using var tx1 = conn.BeginTransaction(IsolationLevel.Serializable);

// Or set at session level via command
using var setLevel = new SqlCommand("SET TRANSACTION ISOLATION LEVEL SNAPSHOT", conn);
await setLevel.ExecuteNonQueryAsync();
using var tx2 = conn.BeginTransaction();

// Savepoints — partial rollback within a transaction
using var tx = conn.BeginTransaction();
try
{
    using var cmd1 = new SqlCommand("INSERT INTO Audit (Action) VALUES ('Step1')", conn, tx);
    await cmd1.ExecuteNonQueryAsync();

    tx.Save("SavePoint1"); // create savepoint

    using var cmd2 = new SqlCommand("UPDATE Inventory SET Stock = -1 WHERE Id = 1", conn, tx);
    await cmd2.ExecuteNonQueryAsync();

    // Partial rollback to savepoint (cmd1 still committed)
    tx.Rollback("SavePoint1");

    await tx.CommitAsync(); // commits only cmd1
}
catch
{
    await tx.RollbackAsync();
    throw;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 10. EXCEPTIONS & ERROR HANDLING

<br/>

## Q. How do you handle database errors in ADO.NET? What is SqlException?

`SqlException` is thrown by `Microsoft.Data.SqlClient` when SQL Server returns a warning or error. It inherits from `DbException`.

```csharp
public async Task<int> SafeInsertAsync(Product product)
{
    using var conn = new SqlConnection(connectionString);

    try
    {
        await conn.OpenAsync();

        using var cmd = new SqlCommand(
            "INSERT INTO Products (Name, Price) VALUES (@Name, @Price); SELECT SCOPE_IDENTITY();", conn);
        cmd.Parameters.Add("@Name",  SqlDbType.NVarChar, 100).Value = product.Name;
        cmd.Parameters.Add("@Price", SqlDbType.Decimal)       .Value = product.Price;

        return Convert.ToInt32(await cmd.ExecuteScalarAsync());
    }
    catch (SqlException ex)
    {
        // SqlException-specific information
        Console.WriteLine($"SQL Error Number: {ex.Number}");    // SQL Server error code
        Console.WriteLine($"Severity:         {ex.Class}");     // 0-10: info, 11-16: user, 17+: system
        Console.WriteLine($"State:            {ex.State}");
        Console.WriteLine($"Server:           {ex.Server}");
        Console.WriteLine($"Procedure:        {ex.Procedure}"); // SP name if applicable
        Console.WriteLine($"Line:             {ex.LineNumber}");
        Console.WriteLine($"Message:          {ex.Message}");

        // Common SQL error numbers
        // 2    — cannot connect (timeout)
        // 208  — invalid object name
        // 547  — foreign key violation
        // 2601 — unique index violation
        // 2627 — primary key violation

        switch (ex.Number)
        {
            case 2627: // PK violation
            case 2601: // unique index violation
                throw new DuplicateKeyException($"Product '{product.Name}' already exists", ex);
            case 547:  // FK violation
                throw new ReferentialIntegrityException("Referenced record not found", ex);
            default:
                throw new DataAccessException("Database error during insert", ex);
        }
    }
    catch (InvalidOperationException ex) when (ex.Message.Contains("connection"))
    {
        throw new DataAccessException("Connection error", ex);
    }
    // Note: using statement guarantees conn.Dispose() even on exception
}

// Iterate multiple errors in SqlException
catch (SqlException ex)
{
    foreach (SqlError error in ex.Errors)
    {
        Console.WriteLine($"  Error {error.Number} (Severity {error.Class}): {error.Message}");
    }
    throw;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Where should you close a connection when an exception occurs?

```csharp
// WRONG — connection not closed if exception occurs before finally
void BadPattern()
{
    var conn = new SqlConnection(connectionString);
    conn.Open();
    // exception here → conn never closed → connection leak
    var cmd = new SqlCommand("SELECT 1/0", conn); // division by zero
    cmd.ExecuteNonQuery();
    conn.Close(); // never reached!
}

// CORRECT — using statement (equivalent to try/finally)
async Task GoodPattern()
{
    // Connection is ALWAYS closed, even on exception
    await using var conn = new SqlConnection(connectionString);
    await conn.OpenAsync();

    await using var cmd = new SqlCommand("SELECT Id FROM Products WHERE Id = @Id", conn);
    cmd.Parameters.Add("@Id", SqlDbType.Int).Value = 1;

    var result = await cmd.ExecuteScalarAsync();
} // conn.Dispose() → Close() → returned to pool

// CORRECT — explicit try/finally (old-style)
async Task ExplicitPattern()
{
    SqlConnection? conn = null;
    try
    {
        conn = new SqlConnection(connectionString);
        await conn.OpenAsync();
        // ... commands
    }
    catch (SqlException ex)
    {
        Console.WriteLine($"DB Error: {ex.Message}");
        throw;
    }
    finally
    {
        conn?.Dispose(); // ALWAYS runs
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 11. PERFORMANCE & BEST PRACTICES

<br/>

## Q. What are the best practices for high-performance ADO.NET code?

**1. Use `SqlDataReader` over `DataSet` for read performance**

```csharp
// FAST — streaming, forward-only
public async Task<List<ProductDto>> GetProductsFastAsync()
{
    var result = new List<ProductDto>();

    await using var conn = new SqlConnection(connectionString);
    await conn.OpenAsync();

    await using var cmd = new SqlCommand(
        "SELECT Id, Name, Price FROM Products WHERE IsActive = 1", conn)
    {
        CommandTimeout = 30
    };

    await using var reader = await cmd.ExecuteReaderAsync(
        CommandBehavior.SequentialAccess); // streaming for BLOBs

    // Cache ordinals outside the loop
    int idOrd    = reader.GetOrdinal("Id");
    int nameOrd  = reader.GetOrdinal("Name");
    int priceOrd = reader.GetOrdinal("Price");

    while (await reader.ReadAsync())
    {
        result.Add(new ProductDto(
            reader.GetInt32(idOrd),
            reader.GetString(nameOrd),
            reader.GetDecimal(priceOrd)));
    }
    return result;
}
```

**2. Explicit `SqlDbType` instead of `AddWithValue`**

```csharp
// BAD — implicit type inference, plan cache pollution
cmd.Parameters.AddWithValue("@Name", "Widget");

// GOOD — explicit type, consistent execution plans
cmd.Parameters.Add("@Name", SqlDbType.NVarChar, 100).Value = "Widget";
```

**3. Async all the way**

```csharp
// Use async versions throughout
await conn.OpenAsync(ct);
await cmd.ExecuteNonQueryAsync(ct);
await cmd.ExecuteReaderAsync(ct);
await cmd.ExecuteScalarAsync(ct);
await reader.ReadAsync(ct);
```

**4. Batch inserts with table-valued parameters or SqlBulkCopy**

```csharp
// SqlBulkCopy — fastest way to insert large volumes
using var bulk = new SqlBulkCopy(connectionString)
{
    DestinationTableName = "Products",
    BatchSize            = 1000,
    BulkCopyTimeout      = 300
};

bulk.ColumnMappings.Add("Name",  "Name");
bulk.ColumnMappings.Add("Price", "Price");
bulk.ColumnMappings.Add("Stock", "Stock");

await bulk.WriteToServerAsync(productsDataTable);
// Or from IDataReader:
await bulk.WriteToServerAsync(reader);
```

**5. Connection pooling — always dispose**

```csharp
// Each using block returns connection to pool
for (int i = 0; i < 100; i++)
{
    await using var conn = new SqlConnection(connectionString);
    await conn.OpenAsync(); // reuses pooled connection
    // ... single unit of work
} // returned to pool → not physically closed
```

**6. Optimize SELECT — return only needed columns**

```csharp
// BAD
var cmd = new SqlCommand("SELECT * FROM Products", conn);

// GOOD — only what you need
var cmd = new SqlCommand("SELECT Id, Name, Price FROM Products WHERE IsActive = 1", conn);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is `CommandBehavior` and when do you use it?

```csharp
// CommandBehavior flags control reader behavior
using var reader = await cmd.ExecuteReaderAsync(
    CommandBehavior.CloseConnection |    // Close connection when reader is closed
    CommandBehavior.SingleRow       |    // Hint: only one row expected (optimization)
    CommandBehavior.SequentialAccess     // Columns must be read left-to-right (BLOB streaming)
);

// CloseConnection — useful in repository methods that return a reader
public static SqlDataReader GetReaderWithAutoClose(string sql, SqlConnection conn)
{
    conn.Open();
    var cmd = new SqlCommand(sql, conn);
    return cmd.ExecuteReader(CommandBehavior.CloseConnection);
    // Caller reads then disposes reader → connection auto-closed
}

// SequentialAccess — for reading large binary/text columns
using var reader = await cmd.ExecuteReaderAsync(CommandBehavior.SequentialAccess);
while (await reader.ReadAsync())
{
    // Must read columns left-to-right when SequentialAccess is used
    long bytesRead;
    var buffer = new byte[4096];
    using var ms = new MemoryStream();
    long dataIndex = 0;
    while ((bytesRead = reader.GetBytes(0, dataIndex, buffer, 0, buffer.Length)) > 0)
    {
        ms.Write(buffer, 0, (int)bytesRead);
        dataIndex += bytesRead;
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 12. ADO.NET vs EF vs DAPPER

<br/>

## Q. What is the difference between ADO.NET, Entity Framework Core, and Dapper?

| Aspect                | ADO.NET (raw)              | Dapper                       | Entity Framework Core             |
|-----------------------|----------------------------|------------------------------|-----------------------------------|
| **Abstraction level** | Low (manual everything)    | Thin micro-ORM               | High (full ORM)                   |
| **Performance**       | Fastest                    | Near ADO.NET                 | Slower (query translation, tracking) |
| **SQL control**       | Full control               | Full control                 | Limited (can use raw SQL)         |
| **Mapping**           | Manual (`reader.GetString`) | Automatic (reflection)      | Automatic (+ conventions)         |
| **Migrations**        | Manual                     | Manual                       | Built-in                          |
| **Learning curve**    | Low                        | Low                          | High                              |
| **Code amount**       | High (verbose)             | Low                          | Lowest                            |
| **Best for**          | Max performance, reporting  | APIs, microservices          | CRUD-heavy enterprise apps        |

```csharp
// Same query: "Get customer by ID" — three approaches

// 1. ADO.NET — full control, most verbose
public async Task<Customer?> GetByIdAdoNet(int id)
{
    await using var conn = new SqlConnection(connectionString);
    await conn.OpenAsync();
    await using var cmd  = new SqlCommand(
        "SELECT Id, Name, Email, CreatedAt FROM Customers WHERE Id = @Id", conn);
    cmd.Parameters.Add("@Id", SqlDbType.Int).Value = id;
    await using var reader = await cmd.ExecuteReaderAsync();
    if (!await reader.ReadAsync()) return null;
    return new Customer
    {
        Id        = reader.GetInt32(0),
        Name      = reader.GetString(1),
        Email     = reader.GetString(2),
        CreatedAt = reader.GetDateTime(3)
    };
}

// 2. Dapper — concise, still uses raw SQL
// Install: dotnet add package Dapper
public async Task<Customer?> GetByIdDapper(int id)
{
    await using var conn = new SqlConnection(connectionString);
    return await conn.QuerySingleOrDefaultAsync<Customer>(
        "SELECT Id, Name, Email, CreatedAt FROM Customers WHERE Id = @Id",
        new { Id = id });
}

// 3. Entity Framework Core — no SQL, LINQ-based
public async Task<Customer?> GetByIdEF(int id)
    => await _context.Customers
        .AsNoTracking()
        .FirstOrDefaultAsync(c => c.Id == id);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. When should you choose ADO.NET over EF Core or Dapper?

```csharp
// Choose ADO.NET when:

// 1. MAXIMUM PERFORMANCE required — bulk operations, ETL, reporting
public async Task BulkLoadProductsAsync(IEnumerable<Product> products)
{
    using var bulk = new SqlBulkCopy(connectionString);
    bulk.DestinationTableName = "Products";

    var table = new DataTable();
    table.Columns.Add("Name",  typeof(string));
    table.Columns.Add("Price", typeof(decimal));

    foreach (var p in products)
        table.Rows.Add(p.Name, p.Price);

    await bulk.WriteToServerAsync(table);
    // Inserts 100,000 rows in seconds — impossible to match with EF Core
}

// 2. Complex stored procedures with OUTPUT params
var (orderId, number, code) = await CreateOrderAsync(customerId, amount);

// 3. Fine-grained connection/transaction control
// 4. Targeting a DB not well-supported by EF Core
// 5. Minimal dependency — no ORM overhead in microservices

// Choose EF Core when:
// - Rapid CRUD development
// - Code-first migrations needed
// - Team prefers LINQ over SQL
// - Domain model / DDD focus

// Choose Dapper when:
// - Performance close to ADO.NET needed
// - Full SQL control but automatic mapping
// - Existing stored procedures / complex SQL
// - Lightweight microservice

// Hybrid approach (common in enterprise)
public class ProductRepository
{
    // Reads — Dapper (concise, fast)
    public Task<IEnumerable<Product>> GetAllAsync()
        => _conn.QueryAsync<Product>("SELECT * FROM Products WHERE IsActive = 1");

    // Writes — EF Core (change tracking, validation)
    public async Task<int> CreateAsync(Product product)
    {
        _context.Products.Add(product);
        await _context.SaveChangesAsync();
        return product.Id;
    }

    // Bulk operations — ADO.NET
    public Task BulkImportAsync(DataTable data)
    {
        using var bulk = new SqlBulkCopy(_connectionString);
        bulk.DestinationTableName = "Products";
        return bulk.WriteToServerAsync(data);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the complete CRUD example using ADO.NET?

```csharp
public class ProductRepository
{
    private readonly string _connectionString;

    public ProductRepository(string connectionString)
    {
        _connectionString = connectionString;
    }

    // CREATE
    public async Task<int> CreateAsync(Product product, CancellationToken ct = default)
    {
        await using var conn = new SqlConnection(_connectionString);
        await conn.OpenAsync(ct);

        await using var cmd = new SqlCommand(
            @"INSERT INTO Products (Name, Price, Stock, CategoryId, CreatedAt)
              VALUES (@Name, @Price, @Stock, @CategoryId, GETUTCDATE());
              SELECT SCOPE_IDENTITY();", conn);

        cmd.Parameters.Add("@Name",       SqlDbType.NVarChar, 200).Value = product.Name;
        cmd.Parameters.Add("@Price",      SqlDbType.Decimal)       .Value = product.Price;
        cmd.Parameters.Add("@Stock",      SqlDbType.Int)            .Value = product.Stock;
        cmd.Parameters.Add("@CategoryId", SqlDbType.Int)            .Value = (object?)product.CategoryId ?? DBNull.Value;

        return Convert.ToInt32(await cmd.ExecuteScalarAsync(ct));
    }

    // READ ALL
    public async Task<List<Product>> GetAllAsync(CancellationToken ct = default)
    {
        var products = new List<Product>();

        await using var conn = new SqlConnection(_connectionString);
        await conn.OpenAsync(ct);

        await using var cmd    = new SqlCommand(
            "SELECT Id, Name, Price, Stock, CategoryId FROM Products WHERE IsDeleted = 0 ORDER BY Name", conn);
        await using var reader = await cmd.ExecuteReaderAsync(ct);

        int idOrd    = reader.GetOrdinal("Id");
        int nameOrd  = reader.GetOrdinal("Name");
        int priceOrd = reader.GetOrdinal("Price");
        int stockOrd = reader.GetOrdinal("Stock");
        int catOrd   = reader.GetOrdinal("CategoryId");

        while (await reader.ReadAsync(ct))
        {
            products.Add(new Product
            {
                Id         = reader.GetInt32(idOrd),
                Name       = reader.GetString(nameOrd),
                Price      = reader.GetDecimal(priceOrd),
                Stock      = reader.GetInt32(stockOrd),
                CategoryId = reader.IsDBNull(catOrd) ? null : reader.GetInt32(catOrd)
            });
        }
        return products;
    }

    // READ BY ID
    public async Task<Product?> GetByIdAsync(int id, CancellationToken ct = default)
    {
        await using var conn = new SqlConnection(_connectionString);
        await conn.OpenAsync(ct);

        await using var cmd = new SqlCommand(
            "SELECT Id, Name, Price, Stock, CategoryId FROM Products WHERE Id = @Id AND IsDeleted = 0", conn);
        cmd.Parameters.Add("@Id", SqlDbType.Int).Value = id;

        await using var reader = await cmd.ExecuteReaderAsync(CommandBehavior.SingleRow, ct);
        if (!await reader.ReadAsync(ct)) return null;

        return new Product
        {
            Id         = reader.GetInt32(0),
            Name       = reader.GetString(1),
            Price      = reader.GetDecimal(2),
            Stock      = reader.GetInt32(3),
            CategoryId = reader.IsDBNull(4) ? null : reader.GetInt32(4)
        };
    }

    // UPDATE
    public async Task<bool> UpdateAsync(Product product, CancellationToken ct = default)
    {
        await using var conn = new SqlConnection(_connectionString);
        await conn.OpenAsync(ct);

        await using var cmd = new SqlCommand(
            @"UPDATE Products
              SET Name = @Name, Price = @Price, Stock = @Stock, CategoryId = @CategoryId, UpdatedAt = GETUTCDATE()
              WHERE Id = @Id AND IsDeleted = 0", conn);

        cmd.Parameters.Add("@Id",         SqlDbType.Int)             .Value = product.Id;
        cmd.Parameters.Add("@Name",       SqlDbType.NVarChar, 200)   .Value = product.Name;
        cmd.Parameters.Add("@Price",      SqlDbType.Decimal)          .Value = product.Price;
        cmd.Parameters.Add("@Stock",      SqlDbType.Int)              .Value = product.Stock;
        cmd.Parameters.Add("@CategoryId", SqlDbType.Int)              .Value = (object?)product.CategoryId ?? DBNull.Value;

        int rows = await cmd.ExecuteNonQueryAsync(ct);
        return rows > 0;
    }

    // DELETE (soft delete)
    public async Task<bool> DeleteAsync(int id, CancellationToken ct = default)
    {
        await using var conn = new SqlConnection(_connectionString);
        await conn.OpenAsync(ct);

        await using var cmd = new SqlCommand(
            "UPDATE Products SET IsDeleted = 1, DeletedAt = GETUTCDATE() WHERE Id = @Id AND IsDeleted = 0", conn);
        cmd.Parameters.Add("@Id", SqlDbType.Int).Value = id;

        return await cmd.ExecuteNonQueryAsync(ct) > 0;
    }
}

public class Product
{
    public int      Id         { get; set; }
    public string   Name       { get; set; } = string.Empty;
    public decimal  Price      { get; set; }
    public int      Stock      { get; set; }
    public int?     CategoryId { get; set; }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>
