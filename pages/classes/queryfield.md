---
layout: page
title: "QueryField (RepoDb)"
permalink: /class/queryfield
tags: [repodb, class, queryfield, orm, hybrid-orm, sqlserver, sqlite, mysql, postgresql]
---

## QueryField

This is used as the field on the query expression. Usually refers to a specific field at the `WHERE` statement of the SQL statement.

It contains the actual [Field](/class/field), [Operation](/enumeration/operation) and [Parameter](/class/parameter) objects as the properties for equations.

By using this class, it would increase the performance of your application as the library's core implementation is very dependent on the tree structuring of the query objects.

> In this tutorial, we will use the `SQL Server` as the database and `C#` as the programming language.

#### Creating an Instance

Below is way on how to create an instance of a query field.

```csharp
var field = new QueryField("Id", 10045);
```

Or, you can also use define the operation.

```csharp
var field = new QueryField("CreatedDateUtc", Operation.GreaterThanOrEqual, DateTime.UtcNow.Date.AddDays(-1));
```

#### Converting to an Enumerable

You can call the `AsEnumerable()` method to convert the instance of this class to `IEnumerable<QueryField>`.

```csharp
var fields = new QueryField("CreatedDateUtc", Operation.GreaterThanOrEqual, DateTime.UtcNow.Date.AddDays(-1)).AsEnumerable();
```

#### Use-Cases

This can be very useful if you are running a query in a dynamic way and if you would like to manage the tree structure of your expression.

```csharp
using (var connection = new SqlConnection(connectionString))
{
    var where = new []
    {
        new QueryField("LastName", Operation.Like, "Doe%"),
        new QueryField("State", Operation.Equal, "Michigan"),
        new QueryField("Age", Operation.Between, new [] (20, 40))
    };
    var people = connection.Query<Person>(where);
    // Do the stuffs for 'people' here
}
```

Or in the update operations (for targetted columns).

```csharp
using (var connection = new SqlConnection(connectionString))
{
    var where = new []
    {
        new QueryField("State", Operation.Equal, "Michigan"),
        new QueryField("Age", Operation.Between, new [] (20, 40))
    };
    var person = new
    {
        IsActive = true,
        LastUpdatedUtc = DateTime.UtcNow
    };
    var updatedRows = connection.Update("[dbo].[Person]", person, where);
}
```

Or even in the delete operations.

```csharp
using (var connection = new SqlConnection(connectionString))
{
    var where = new []
    {
        new QueryField("State", Operation.Equal, "Michigan"),
        new QueryField("Age", Operation.Between, new [] (20, 40))
    };
    var deletedRows = connection.Delete<Person>(where);
}
```

#### Retrieving the Operation Text

To retrieve the text of the operation, simply call the `GetOperationText()` method.

```csharp
var field = new QueryField("CreatedDateUtc", Operation.GreaterThanOrEqual, DateTime.UtcNow.Date.AddDays(-1));
var operation = field.GetOperationText();
```

#### Reusability

We sometimes have a scenario to reuse the instance of query field to avoid creating multiple instance of the same expression. To reuse the instance, you have to call the `Reset()` method first.

```csharp
using (var connection = new SqlConnection(connectionString))
{
    var where = new []
    {
        new QueryField("LastName", Operation.Like, "Doe%"),
        new QueryField("State", Operation.Equal, "Michigan"),
        new QueryField("Age", Operation.Between, new [] (20, 40))
    };
    var people = connection.Query<Person>(where);
    
    // Do the stuffs for 'people' here

    // Reset here
    where.Reset();

    // Reuse it here
    var customers = connection.Query<Customer>(where);

    // Do the stuffs for 'customers' here
}
```

> You can also call the `Reset()` method on an instance basis.

#### IsForUpdate Method

There is a scenario that we are using the query field for the purpose of updates.

Let us say, you have a person named `John Doe` and you would like to update this person's name to `James Doe` using the name qualifier. See the translated SQL below.

```csharp
UPDATE [dbo].[Person] SET Name = 'James Doe' WHERE Name = 'John Doe';
```

To make a parameterized statement for this, we need to have a SQL like below.

```csharp
UPDATE [dbo].[Person] SET Name = @Name WHERE Name = @_Name;
```

Where the value of `@Name` is `James Doe` and the value of `@_Name` is `John Doe`.

If you create a query field like below.

```csharp
var field = new QueryField("Name", "John Doe");
```

By default, the name of the paremter to be passed for `Name` query field is `@Name`. If you have an entity to be updated where there is also a `Name` property, then they are colliding.

To fix this issue, you have to call the `IsForUpdate()` explicitly. By calling this method, the `Name` property will be prepended by an underscore (`_`) character before the actual execution.