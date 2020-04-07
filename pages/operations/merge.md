---
layout: navpage
sidebar: operations
title: "Merge (RepoDb)"
permalink: /operation/merge
tags: [repodb, tutorial, merge, orm, hybrid-orm, sqlserver, sqlite, mysql, postgresql]
---

# Merge

This method is used to merge a record in the database.

> This operation inserts a new record in the database if the target record is not yet existing, otherwise, it will update the existing one.

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

Or visit our [installation](/tutorial/installation) page for more information.

> In this tutorial, we will use the SQL Server as the database and C# as the programming language.

#### Learnings

Below is a sample code to merge a row into the `[dbo].[Person]` table.

```csharp
using (var connection = new SqlConnection(connectionString))
{
	var person = new Person
	{
		Id = 10045,
		Name = "John Doe",
		Address = "New York",
		DateOfBirth = DateTime.Parse("2020-01-01"),
		IsActive = true,
		DateInsertedUtc = DateTime.UtcNow
	};
	var id = connection.Merge<Person>(person);
}
```

By default, it uses the primary (or identity) field as the qualifier. You can override by simply passing the list of fields in the `qualifiers` argument.

```csharp
using (var connection = new SqlConnection(connectionString))
{
	var person = new Person
	{
		Id = 10045,
		Name = "John Doe",
		Address = "New York",
		DateOfBirth = DateTime.Parse("2020-01-01"),
		IsActive = true,
		DateInsertedUtc = DateTime.UtcNow
	};
	var id = connection.Merge<Person>(person,
		qualifiers: Field.From("Name", "DateOfBirth"));
}
```

> The result is always the value of primary key. If the primary key is identity, and a new record has been inserted, then the newly generated identity value will be returned.

#### Targetting a Table

You can also target a specific table by passing the literal table and dynamic object like below.

```csharp
using (var connection = new SqlConnection(connectionString))
{
	var person = new
	{
		Id = 10045,
		Name = "John Doe",
		Address = "New York",
		DateOfBirth = DateTime.Parse("2020-01-01"),
		IsActive = true,
		DateInsertedUtc = DateTime.UtcNow
	};
	var id = connection.Merge("[dbo].[Person]",
		entity: person);
}
```

#### Table Hints

To pass a hint, simply write the table-hints and pass it in the `hints` argument.

```csharp
using (var connection = new SqlConnection(connectionString))
{
	var id = connection.Merge<Person>(person,
		hints: "WITH (TABLOCK)");
}
```

Or, you can use the [SqlServerTableHints](/class/sqlservertablehints) class.

```csharp
using (var connection = new SqlConnection(connectionString))
{
	var id = connection.Merge<Person>(person,
		hints: SqlServerTableHints.TabLock);
}
```
