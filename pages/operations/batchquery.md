---
layout: navpage
sidebar: operations
title: "BatchQuery"
permalink: /operation/batchquery
tags: [repodb, tutorial, batchquery, orm, hybrid-orm, sqlserver, sqlite, mysql, postgresql]
---

# BatchQuery

This method is used to query the rows from the database by batch.

#### Data Providers

Below are the supported data providers by this operation.

- [SQL Server](https://www.nuget.org/packages/RepoDb.SqlServer)
- [SQLite](https://www.nuget.org/packages/RepoDb.SqLite)
- [MySQL](https://www.nuget.org/packages/RepoDb.MySql)
- [PostgreSQL](https://www.nuget.org/packages/RepoDb.PostgreSql)

#### Installation

To install, simply type the code snippets below in your Package Manager Console.

```csharp
> Install-Package RepoDb.SqlServer
```

Then call the bootstrapper once.

```csharp
RepoDb.SqlServerBootstrap.Initialize();
```

Or visit our [installation](/tutorial/installation) page for more information.

> In this tutorial, we will use the SQL Server as the database and C# as the programming language.

#### Learnings

Below is a sample code that query the 1st 20 batch of active rows from the `[dbo].[Person]` table based on the date creation.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
    var orderBy = OrderField.Parse(new { DateInsertedUtc = Order.Descending });
    var page = 0; // Starts at 0 for the first batch
    var rowsPerBatch = 20;

    var people = connection.BatchQuery<Person>(page: page,
        rowsPerBatch: rowsPerBatch,
        orderBy: orderBy,
        where: e => e.IsActive == true);
}
```

> Please be aware that the page is starting at `0`.

And below is a sample code that queries the 3rd batch.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
	var orderBy = OrderField.Parse(new { DateInsertedUtc = Order.Descending });
    var page = 2;// This is the 3rd batch
    var rowsPerBatch = 20;

	var people = connection.BatchQuery<Person>(page: page,
		rowsPerBatch: rowsPerBatch,
		orderBy: orderBy,
		where: e => e.IsActive == true);
}
```

#### Targetting a Table

You can also target a specific table by passing the literal table and field name like below.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
	var orderBy = OrderField.Parse(new { DateInsertedUtc = Order.Descending });
    var page = 0;
    var rowsPerBatch = 20;

	var people = connection.BatchQuery("Person",
		page: page,
		rowsPerBatch: rowsPerBatch,
		orderBy: orderBy,
		where: new { IsActive = true });
}
```

#### Specific Columns

You can also query specific columns by passing the list of fields.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
	var orderBy = OrderField.Parse(new { DateInsertedUtc = Order.Descending });
    var page = 0;
    var rowsPerBatch = 20;

	var people = connection.BatchQuery("Person",
		page: page,
		rowsPerBatch: rowsPerBatch,
		orderBy: orderBy,
		where: new { IsActive = true },
		fields: Field.From("Id", "Name", "DateInsertedUtc"));
}
```

#### Filtering the Results

You can also use the [QueryGroup](/class/querygroup) or [QueryField](/class/queryfield) if you are to enhance the `WHERE` expressions.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
	var where = new []
	{
		new QueryField("IsActive", true),
		new QueryField("DateInsertedUtc", Operation.GreaterThanOrEqual, DateTime.UtcNow.Date.AddDays(-1))
	};
	var orderBy = OrderField.Parse(new { DateInsertedUtc = Order.Descending })
    var page = 0;
    var rowsPerBatch = 20;

	var people = connection.BatchQuery("Person",
		page: page,
		rowsPerBatch: rowsPerBatch,
		orderBy: orderBy,
		where: where);
}
```

#### Table Hints

You can also pass a hint.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
	var orderBy = OrderField.Parse(new { DateInsertedUtc = Order.Descending });
    var page = 0;
    var rowsPerBatch = 20;

	var people = connection.BatchQuery<Person>(page: page,
		rowsPerBatch: rowsPerBatch,
		orderBy: orderBy,
		where: e => e.IsActive == true,
		hints: "WITH (NOLOCK)");
}
```

Or, you can use the [SqlServerTableHints](/classes[SqlServerTableHints](/class/sqlservertablehints)) class.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
	var orderBy = OrderField.Parse(new { DateInsertedUtc = Order.Descending });
    var page = 0;
    var rowsPerBatch = 20;

	var people = connection.BatchQuery<Person>(page: page,
		rowsPerBatch: rowsPerBatch,
		orderBy: orderBy,
		where: e => e.IsActive == true,
		hints: SqlServerTableHints.NoLock);
}
```
