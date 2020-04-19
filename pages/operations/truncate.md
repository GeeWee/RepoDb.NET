---
layout: navpage
sidebar: operations
title: "Truncate (RepoDb)"
permalink: /operation/truncate
tags: [repodb, tutorial, count, orm, hybrid-orm, sqlserver, sqlite, mysql, postgresql]
---

# Truncate

This method is used to truncate all the rows on a single table from the database.

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

#### Leanings

Below is a sample code that truncates the `[dbo].[Person]` table.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
	connection.Truncate<Person>();
}
```

#### Targetting a Table

You can also target a specific table by passing the literal table and field name like below.

```csharp
using (var connection = new SqlConnection(connectionString).EnsureOpen())
{
	connection.Truncate("[dbo].[Person]");
}
```
