---
layout: navpage
sidebar: features
title: "Implicit Mapping"
description: "This is the feature that would allow you to implicitly map the .NET CLR objects or class properties into its equivalent object in the database. Certain mapper classes has been provided in order to avoid the usage of the attributes in the classes."
permalink: /feature/implicitmapping
tags: [repodb, class, implicitmapping, orm, hybrid-orm, sqlserver, sqlite, mysql, postgresql]
---

# Implicit Mapping

This is the feature that would allow you to implicitly map the .NET CLR objects or class properties into its equivalent object in the database. Certain mapper classes has been provided in order to avoid the usage of the attributes in the classes.

Below are the list of mappers that can be used.

- [ClassMapper](/mapper/classmapper)
- [IdentityMapper](/mapper/identitymapper)
- [PrimaryMapper](/mapper/primarymapper)
- [PropertyHandlerMapper](/mapper/propertyhandlermapper)
- [PropertyMapper](/mapper/propertymapper)
- [TypeMapper](/mapper/typemapper)

Below are the list of cachers that can be used.

- [ClassMappedNameCache](/cacher/classmappednamecache)
- [IdentityCache](/cacher/identitycache)
- [PrimaryCache](/cacher/primarycache)
- [PropertyHandlerCache](/cacher/propertyhandlercache)
- [PropertyMappedNameCache](/cacher/propertymappednamecache)
- [TypeMapCache](/cacher/typemapcache)

> Use the [FluentMapper](/mapper/fluentmapper) class to simplify the implementation of the entity mappings.

#### Class Name Mapping

Use the [ClassMapper](/mapper/classmapper) class to manage the mappings of the class into its equivalent object in the database (ie: Table, View).

Let us say you have a `Customer` class which has an equivalent table named `Customer` under `sales` schema.

```csharp
public class Customer
{
    public int Id { get; set; }
    public string FirstName { get; set;}
    public string LastName { get; set;}
    ...
}
```

Then use the `Add()` method to map it.

```csharp
ClassMapper.Add<Customer>("[sales].[Customer]");
```
To retrieve the mapped name, use the `Get()` method.

```csharp
var mappedName = ClassMapper.Get<Customer>();
```

We highly recommend to use the [ClassMappedNameCache](/cacher/classmappednamecache) class when retrieving the mapped name to maximize the reusability and performance.

```csharp
var mappedName = ClassMappedNameCache.Get<Customer>();
```

To remove the mapping, use the `Remove()` method.

```csharp
ClassMapper.Remove<Customer>();
```

#### Identity Mapping

Use the [IdentityMapper](/mapper/identitymapper) class to manage the mappings of the class property as an identity.

To add a mapping, use the `Add()` method.

```csharp
IdentityMapper.Add<Customer>(e => e.Id);
```

To retrieve the mapping, use the `Get()` method. The method returns an instance [ClassProperty](/class/classproperty) object.

```csharp
var property = IdentityMapper.Get<Customer>();
```

We highly recommend to use the [IdentityCache](/cacher/identitycache) class when retrieving the identity property to maximize the reusability and performance.

```csharp
var property = IdentityCache.Get<Customer>();
```

To remove the mapping, use the `Remove()` method.

```csharp
IdentityMapper.Remove<Customer>();
```

#### Primary Mapping

Use the [PrimaryMapper](/mapper/primarymapper) class to manage the mappings of the class property as a primary.

To add a mapping, use the `Add()` method.

```csharp
PrimaryMapper.Add<Customer>(e => e.Id);
```
To retrieve the mapping, use the `Get()` method. The method returns an instance [ClassProperty](/class/classproperty) object.

```csharp
var property = PrimaryMapper.Get<Customer>();
```

We highly recommend to use the [PrimaryCache](/cacher/primarycache) class when retrieving the primary property to maximize the reusability and performance.

```csharp
var mappedName = PrimaryCache.Get<Customer>();
```

To remove the mapping, use the `Remove()` method.

```csharp
PrimaryMapper.Remove<Customer>();
```

#### PropertyHandler Mapping

Use the [PropertyHandlerMapper](/mapper/propertyhandlermapper) class to manage the mappings between the property handles and the .NET CLR type or class property.

###### Property Level

Let us say you have property handler below.

```csharp
public class CustomerAddressPropertyHandler : IPropertyHandler<string, Address>
{
    public Address Get(string input, ClassProperty property)
    {
        return !string.IsNullOrEmpty(input) ? JsonConvert.Deserialize<Address>(input) : null;
    }

    public string Set(Address input, ClassProperty property)
    {
        return input != null ? JsonConvert.Serialize(input) : null;
    }
}
```

To add a mapping, use the `Add()` method.

```csharp
PropertyHandlerMapper.Add<Customer, CustomerAddressPropertyHandler>(e => e.Address);
```

To retrieve the mapping, use the `Get()` method. The method returns an instance [ClassProperty](/class/classproperty) object.

```csharp
var propertyHandler = PropertyHandlerMapper.Get<Customer, CustomerAddressPropertyHandler>(e => e.Address);
```

We highly recommend to use the [PropertyHandlerCache](/cacher/propertyhandlercache) class when retrieving the cached property handlers to maximize the reusability and performance.

```csharp
var propertyHandler = PropertyHandlerMapper.Get<Customer, CustomerAddressPropertyHandler>(e => e.Address);
```

To remove the mapping, use the `Remove()` method.

```csharp
PropertyHandlerMapper.Remove<Customer>(e => e.Address);
```

###### Type Level

Let us say you want to handle all the `System.DateTime` properties and you have created the property handler below.

```csharp
public class DateTimeKindToUtcPropertyHandler : IPropertyHandler<datetime?, datetime?>
{
    public datetime? Get(datetime? input, ClassProperty property)
    {
        return input.HasValue ? DateTime.SpecifyKind(input.Value, Kind.Utc) : null;
    }

    public datetime? Set(datetime? input, ClassProperty property)
    {
        return input.HasValue ? DateTime.SpecifyKind(input.Value, Kind.Unspecified) : null;
    }
}
```

To add a mapping, use the type-level `Add()` method.

```csharp
PropertyHandlerMapper.Add<DateTime, DateTimeKindToUtcPropertyHandler>();
```

To retrieve the mapping, use the type-level `Get()` method.

```csharp
var propertyHandler = PropertyHandlerMapper.Get<DateTime>();
```

Same here, we highly recommend to use the [PropertyHandlerCache](/cacher/propertyhandlercache) class instead.

```csharp
var propertyHandler = PropertyHandlerMapper.Get<DateTime>();
```

To remove the mapping, use the type-level `Remove()` method.

```csharp
PropertyHandlerMapper.Remove<DateTime>();
```

#### Property Name Mapping

Use the [PropertyMapper](/mapper/propertymapper) class to manage the mappings of the class property into its equivalent column in the database.

Let us say you have the  `Customer` class below.

```csharp
public class Customer
{
    public int Id { get; set; }
    public string FirstName { get; set;}
    public string LastName { get; set;}
    public DateTime DateOfBirth { get; set;}
    ...
}
```

And the `[dbo].[Customer]` table below.

```csharp
CREATE TABLE [dbo].[Customer]
(
	[Id] [bigint] IDENTITY(1,1) NOT NULL,
	[FName] [nvarchar](128) NOT NULL,
	[LName] [nvarchar](128) NOT NULL,
	[DOB] [datetime2](5) NOT NULL,
	[CreatedDateUtc] [datetime2](5) NOT NULL,
	CONSTRAINT [CRIX_Customer_Id] PRIMARY KEY CLUSTERED ([Id] ASC) ON [PRIMARY]
)
ON [PRIMARY];
GO
```

Then use the `Add()` method to map it.

```csharp
PropertyMapper.Add<Customer>(e => e.FirstName, "[FName]");
PropertyMapper.Add<Customer>(e => e.LastName, "[LName]");
PropertyMapper.Add<Customer>(e => e.DateOfBirth, "[DOB]");
```

To retrieve the mapped name, use the `Get()` method.

```csharp
var firstName = PropertyMapper.Get<Customer>(e => e.FirstName);
var lastName = PropertyMapper.Get<Customer>(e => e.LastName);
var dob = PropertyMapper.Get<Customer>(e => e.DateOfBirth);
```

We highly recommend to use the [PropertyMappedNameCache](/cacher/propertymappednamecache) class when retrieving the cached mapped name to maximize the reusability and performance.

```csharp
var firstName = PropertyMappedNameCache.Get<Customer>(e => e.FirstName);
var lastName = PropertyMappedNameCache.Get<Customer>(e => e.LastName);
var dob = PropertyMappedNameCache.Get<Customer>(e => e.DateOfBirth);
```

To remove the mapping, use the `Remove()` method.

```csharp
PropertyMapper.Remove<Customer>(e => e.FirstName);
PropertyMapper.Remove<Customer>(e => e.LastName);
PropertyMapper.Remove<Customer>(e => e.DateOfBirth);
```

#### Database Type Mapping

Use the [TypeMapper](/mapper/typemapper) class to manage the mappings of the .NET CLR type or class property into its equivalent database type.

###### Property Level

Let us say you have the `Customer` class below.

```csharp
public class Customer
{
    public int Id { get; set; }
    ...
    public DateTime DateOfBirth { get; set; }
    ...
}
```

And you would like to map the property of the `DateOfBirth` property into `DbType.DateTime2` database type.

Then, use the `Add()` method to map it.

```csharp
TypeMapper.Add<Customer>(e => e.DateOfBirth, DbType.DateTime2);
```

To retrieve the mapping, use the `Get()` method.

```csharp
var dbType = TypeMapper.Get<Customer>(e => e.DateOfBirth);
```

We highly recommend to use the [TypeMapCache](/cacher/typemapcache) class when retrieving the cached database types to maximize the reusability and performance.

```csharp
var dbType = TypeMapCache.Get<Customer>(e => e.DateOfBirth);
```

To remove the mapping, ue the `Remove()` method.

```csharp
TypeMapper.Remove<Customer>(e => e.DateOfBirth);
```

###### Type Level

Let us say you would like to always map the `System.DateTime` .NET CLR type equivalent to `DbType.DateTime2` database type.

Then use the type-level mapping via `Add()` method.

```csharp
TypeMapper.Add<DateTime>(DbType.DateTime2);
```

To retrieve the mapping, use the type-level `Get()` method.

```csharp
var dbType = TypeMapper.Get<DateTime>();
```

Same here, we highly recommend to use the [TypeMapCache](/cacher/typemapcache) class instead.

```csharp
var dbType = TypeMapCache.Get<DateTime>();
```

To remove the mapping, use the type-level `Remove()` method.

```csharp
TypeMapper.Remove<DateTime>();
```

Please visit the [Type Mapping](/feature/typemapping) feature for further information.

> In the `Add()` method of all mappers, an exception will be thrown if the mapping is already exists and you passed a `false` value in the `force` argument.


#### Fluent Mapping

Use the [FluentMapper](/mapper/fluentmapper) class to manage the fluent mappings for `Table`, `Column`, `Primary`, `Identity`, `Database Types` and `Property Handlers`.


###### Entity Mapping

To define the mappings for an entity, use the `Entity()` method.

```csharp
FluentMapper
    .Entity<Customer>()
    .Table("[sales].[Customer]")
    .Primary(e => e.Id)
    .Identity(e => e.Id)
    .Column(e => e.FirstName, "[FName]")
    .Column(e => e.LastName, "[LName]")
    .Column(e => e.DateOfBirth, "[DOB]")
    .DbType(e => e.DateOfBirth, DbType.DateTime2)
    .PropertyHandler<CustomerAddressPropertyHandler>(e => e.Address);
```

###### Type-Level Mapping

To define the mappings for a specfic .NET CLR type, use the `Type()` method.

```csharp
FluentMapper
    .Type<DateTime>()
    .DbType(DbType.DateTime2)
    .PropertyHandler<DateTimeKindToUtcPropertyHandler>(new DateTimeKindToUtcPropertyHandler());
```
