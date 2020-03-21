---
layout: page
title: "IDbSetting (RepoDb)"
permalink: /interface/idbsetting
tags: [repodb, class, idbsetting, orm, hybrid-orm, sqlserver, sqlite, mysql, postgresql]
---

## IDbSetting

This is an interface that is used to mark a class to be a database setting object. It allows you to control the behavior of the library based on the value you provided on the properties this class.

#### Properties

Below are the properties available from this interface.

- AreTableHintsSupported - gets the value that indicates whether the table hints are supported.
- ClosingQuote - gets the character used for closing quote.
- AverageableType - gets the default averageable .NET CLR types for the database.
- DefaultSchema - gets the default schema of the database.
- IsDirectionSupported - gets a value that indicates whether setting the value of `DbParameter.Direction` object is supported.
- IsExecuteReaderDisposable - gets a value that indicates whether the `DbCommand` object must be disposed after calling the ``DbCommand.ExecuteReader()` method.
- IsMultiStatementExecutable - gets a value whether the multiple statement execution is supported.
- IsPreparable - gets a value that indicates whether the current DB Provider supports the `DbCommand.Prepare()` calls.
- IsUseUpsert - gets a value that indicates whether the Insert/Update operation will be used for [Merge](/operation/merge) operation.
- OpeningQuote - gets the character used for opening quote.
- ParameterPrefix - gets the character used for the database command parameter prefixing.
- SchemaSeparator - gets the character (or string) used for dot notation.

#### How to Implement?

You have to manually create a class that implements this interface.

```csharp
public class CustomSqlServerDbSetting : IDbSetting
{
        public bool AreTableHintsSupported { get; set; } = true;
        public string ClosingQuote { get; set; } = "]";
        public Type AverageableType { get; set; } = typeof(double);
        public string DefaultSchema { get; set; } = "dbo";
        public bool IsDirectionSupported { get; set; } = true;
        public bool IsExecuteReaderDisposable { get; set; } = true;
        public bool IsMultiStatementExecutable { get; set; } = true;
        public bool IsPreparable { get; set; } = true;
        public bool IsUseUpsert { get; set; } = false;
        public string OpeningQuote { get; set; } = "[";
        public string ParameterPrefix { get; set; } = "@";
        public string SchemaSeparator { get; set; } = ".";
}
```

#### How to Use?

Once the class has been implemented, you have call the [DbSettingMapper](/mapper/dbsettingmapper) class to map this setting per RDBMS data provider.

```csharp
DbSettingMapper.Add(typeof(SqlConnection), new CustomSqlServerDbSetting(), true);
```