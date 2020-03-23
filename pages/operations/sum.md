---
layout: page
title: "Sum (RepoDb)"
permalink: /operation/sum
tags: [repodb, tutorial, sum, orm, hybrid-orm, sqlserver, sqlite, mysql, postgresql]
---

## Sum

This method is used to get the summation value of the column from the database.

#### Data Providers

Below are the supported data providers by this operation.

- [SQL Server](https://www.nuget.org/packages/RepoDb.SqlServer)
- [SQLite](https://www.nuget.org/packages/RepoDb.SqLite)
- [MySQL](https://www.nuget.org/packages/RepoDb.MySql)
- [PostgreSQL](https://www.nuget.org/packages/RepoDb.PostgreSql)

#### Installation

To install, simply type the codes below in your Package Manager Console.

```csharp
> Install-Package RepoDb.SqlServer
```

Then call the bootstrapper once.

```csharp
RepoDb.SqlServerBootstrap.Initialize();
```

Or visit our [installation](/tutorials/installation) page for more information.

#### Learnings

> In this tutorial, we will use the SQL Server as the database and C# as the programming language.

Below is a sample code that gets the summation value of column *Value* from the *[dbo].[Sales]* table for a specific customer since yesterday.

```csharp
using (var connection = new SqlConnection(connectionString))
{
	var customerExpenses = connection.Sum<Sales>(e => e.Value,
		e => e.CustomerId == 10045 && e.DateInsertedUtc >= DateTime.UtcNow.Date.AddDays(-1));
}
```

#### Targetting a Table

You can also target a specific table by passing the literal table and field name like below.

```csharp
using (var connection = new SqlConnection(connectionString))
{
	var customerExpenses = connection.Sum("[dbo].[Sales]", Field.From("Value"), new { State = "Michigan" });
}
```

Or, use the *Query Object(s)* if you are to enhance the *where* expressions.

```csharp
using (var connection = new SqlConnection(connectionString))
{
	var where = new []
	{
		new QueryField("CustomerId", 10045),
		new QueryField("DateInsertedUtc", Operation.GreaterThanOrEqual, DateTime.UtcNow.Date.AddDays(-1))
	}
	var customerExpenses = connection.Sum("[dbo].[Sales]", Field.From("Value"), where: where);
}
```

#### Table Hints

To pass a hint, simply write the table-hints and pass it in the `hints` argument.

```csharp
using (var connection = new SqlConnection(connectionString))
{
	var customerExpenses = connection.Sum<Sales>(e => e.Value,
		e => e.CustomerId == 10045 && e.DateInsertedUtc >= DateTime.UtcNow.Date.AddDays(-1),
		hints: "WITH (NOLOCK)");
}
```

Or, you can use the [SqlServerTableHints](/class/SqlServerTableHints) class.

```csharp
using (var connection = new SqlConnection(connectionString))
{
	var customerExpenses = connection.Sum<Sales>(e => e.CustomerId == 10045 && e.DateInsertedUtc >= DateTime.UtcNow.Date.AddDays(-1),
		hints: SqlServerTableHints.NoLock);
}
```

#### Passing a Transaction

To pass a transaction on this method, simply create an instance of `IDbConnection` and pass it at the `transaction` argument.

```csharp
using (var connection = new SqlConnection(connectionString))
{
	using (var transaction = connection.BeginTransaction())
	{
		try
		{
			var customerExpenses = connection.Sum<Sales>(e => e.Value,
				e => e.CustomerId == 10045 && e.DateInsertedUtc >= DateTime.UtcNow.Date.AddDays(-1),
				transaction: transaction);

			transaction.Commit();
		}
		catch (Exception ex)
		{
			// Handles the 'ex' here
		}
	}
}
```

> To commit the transaction, you have to explicitly call the `Commit()` method.


