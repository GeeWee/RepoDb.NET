---
layout: navpage
sidebar: operations
title: "DeleteAll"
permalink: /operation/deleteall
tags: [repodb, tutorial, count, orm, hybrid-orm, sqlserver, sqlite, mysql, postgresql]
---

# DeleteAll

This method is used to delete all the rows on a single table from the database.

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

#### Leanings

Below is a sample code that delete all the rows from the `[dbo].[Person]` table.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
	var deletedRows = connection.DeleteAll<Person>();
}
```

Or you can target the list of primary keys.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
	var primaryKeys = new [] { 10045, 11921, 12001 }; 
	var deletedRows = connection.DeleteAll<Person>(primaryKeys);
}
```

#### Targetting a Table

You can also target a specific table by passing the literal table name like below.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
	var deletedRows = connection.DeleteAll("[dbo].[Person]");
}
```

#### Table Hints

To pass a hint, simply write the table-hints and pass it in the `hints` argument.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
	var deletedRows = connection.DeleteAll<Person>(hints: "WITH (NOLOCK)");
}
```

Or, you can use the [SqlServerTableHints](/class/sqlservertablehints) class.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
	var deletedRows = connection.DeleteAll<Person>(hints: SqlServerTableHints.TabLock);
}
```
