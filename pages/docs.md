---
layout: page
title: Documentation (RepoDb)
permalink: /docs
---

## Documentation

RepoDb has bunch of things available for you. You can maximize your development by leveraging most of it.

#### Get Started

Please click any of the link below to fast-track your learnings about this library.

- [SqlServer](/tutorials/getting-started)
- [SqLite](/tutorials/getting-started-sqlite)
- [MySql](/tutorials/getting-started-mysql)
- [PostgreSql](/tutorials/getting-started-postgresql)

To install, please visit our [installation](/tutorials/installation) page for more information.

#### Features

Below are the list of features available.

- [Batch Operations](/feature/batch-operations) - in progress
- [Bulk Operations](/feature/bulk-operations) - in progress
- [Caching](/feature/caching) - in progress
- [Connection Persistency](/feature/connectionpersistency)
- [Enumeration](/feature/enumeration)
- [Expression Trees](/feature/expression-trees) - in progress
- [Field/Class Mapping](/feature/field-class-mapping) - in progress
- [Hints](/feature/hints)
- [Multi-Resultset Query](/feature/multi-resultset-query) - in progress
- [Property Handlers](/feature/property-handlers) - in progress
- [Repositories](/feature/repositories) - in progress
- [Tracing](/feature/tracing) - in progress
- [Transaction](/feature/transaction) - in progress
- [Type Mapping](/feature/type-mapping) - in progress

#### Methods

Below are the list of methods available.

###### Execute Methods

Below are the list of execute-methods available. It support all RDBMS data providers.

- [ExecuteNonQuery](/operation/executenonquery)
- [ExecuteQuery](/operation/executequery)
- [ExecuteQueryMultiple](/operation/executequerymultiple)
- [ExecuteReader](/operation/executereader)
- [ExecuteScalar](/operation/executescalar)

###### Fluent Methods

Below are the list of fluent-methods available. It only support the [SQL Server](https://www.nuget.org/packages/RepoDb.SqlServer), [SQLite](https://www.nuget.org/packages/RepoDb.SqLite), [MySQL](https://www.nuget.org/packages/RepoDb.MySql) and  [PostgreSQL](https://www.nuget.org/packages/RepoDb.PostgreSql) RDBMS data providers.

- [Average](/operation/average)
- [AverageAll](/operation/averageall)
- [BatchQuery](/operation/batchquery)
- [BulkDelete](/operation/bulkdelete)
- [BulkInsert](/operation/bulkinsert)
- [BulkMerge](/operation/bulkmerge)
- [BulkUpdate](/operation/bulkupdate)
- [Count](/operation/count)
- [CountAll](/operation/countall)
- [Delete](/operation/delete)
- [DeleteAll](/operation/deleteall)
- [Exists](/operation/exists)
- [Insert](/operation/insert)
- [InsertAll](/operation/insertall)
- [Max](/operation/max)
- [MaxAll](/operation/maxall)
- [Merge](/operation/merge)
- [MergeAll](/operation/mergeall)
- [Min](/operation/min)
- [MinAll](/operation/minall)
- [Query](/operation/query)
- [QueryAll](/operation/queryall)
- [QueryMultiple](/operation/querymultiple)
- [Sum](/operation/sum)
- [SumAll](/operation/sumall)
- [Truncate](/operation/truncate)
- [Update](/operation/update)
- [UpdateAll](/operation/updateall)

#### Classes

Below are the list of classes available.

- [BaseDbSetting](/class/basedbsetting)
- [BaseStatementBuilder](/class/basestatementbuilder)
- [BaseRepository](/class/baserepository)
- [BulkInsertMapItem](/class/bulkinsertmapitem)
- [CacheItem](/class/cacheitem)
- [CancellableTraceLog](/class/cancellabletracelog)
- [ClassExpression](/class/classexpression)
- [ClassProperty](/class/classproperty)
- [DataEntityDataReader](/class/dataentitydatareader)
- [DataReader (Reflection)](/class/datareader)
- [DbRepository](/class/dbrepository)
- [DbField](/class/dbfield)
- [Field](/class/field)
- [MemoryCache](/class/memorycache)
- [OrderField](/class/orderfield)
- [Parameter](/class/parameter)
- [PropertyValue](/class/propertyvalue)
- [QueryBuilder](/class/querybuilder)
- [QueryField](/class/queryfield)
- [QueryGroup](/class/querygroup)
- [QueryMultipleExtractor](/class/querymultipleextractor)
- [TraceLog](/class/tracelog)

###### SqlServer

- [SqlServerTableHints](/class/sqlservertablehints)

#### Attributes

Below are the list of attributes available (per package).

###### Core

- [Identity](/attribute/identity)
- [Map](/attribute/map)
- [Primary](/attribute/primary)
- [PropertyHandler](/attribute/propertyhandler)
- [Text](/attribute/text)
- [TypeMap](/attribute/typemap)

###### SqlServer

- [MicrosoftSqlServerTypeMapAttribute](/attribute/microsoftsqlservertypemap)
- [SystemSqlServerTypeMapAttribute](/attribute/systemsqlservertypemap)

###### MySql

- [MySqlTypeMapAttribute](/attribute/mysqltypemap)

###### PostgreSql

- [NpgsqlTypeMapAttribute](/attribute/npgsqltypemap)

#### Enumerations

- [Conjunction](/enumeration/conjunction)
- [ConnectionPersistency](/enumeration/connectionpersistency)
- [ConversionType](/enumeration/conversiontype)
- [Operation](/enumeration/operation)
- [Order](/enumeration/order)

#### Interfaces

Below are the list of interfaces available.

- [ICache](/interface/icache)
- [IDbHelper](/interface/idbhelper)
- [IDbSetting](/interface/idbsetting)
- [IExpirable](/interface/iexpirable)
- [IPropertyHandler](/interface/ipropertyhandler)
- [IResolver](/interface/iresolver)
- [IStatementBuilder](/interface/istatementbuilder)
- [ITrace](/interface/itrace)

#### Cachers

Below are the list of cache-objects available.

- [ClassMappedNameCache](/cacher/classmappednamecache)
- [CommandTextCache](/cacher/commandtextcache)
- [DbFieldCache](/cacher/dbfieldcache)
- [FieldCache](/cacher/fieldcache)
- [IdentityCache](/cacher/identitycache)
- [PrimaryCache](/cacher/primarycache)
- [PropertyCache](/cacher/propertycache)
- [PropertyMappedNameCache](/cacher/propertymappednamecache)

#### Mappers

Below are the list of mappers available.

- [DbHelperMapper](/mapper/dbhelpermapper)
- [DbSettingMapper](/mapper/dbsettingmapper)
- [PropertyTypeHandlerMapper](/mapper/propertytypehandlermapper)
- [StatementBuilderMapper](/mapper/statementbuildermapper)
- [TypeMapper](/mapper/typemapper)

#### Extensibility

Below are the list of extensibility-objects available. It is useful to support other RDBMS data providers.

- [Average Type Resolver](/extensibility/averagetyperesolver)
- [Convert Field Resolver](/extensibility/convertfieldresolver)
- [Database Helper](/extensibility/databasehelper)
- [Database Setting](/extensibility/databasesetting)
- [Folder Structuring](/extensibility/folderstructuring)
- [Statement Builder](/extensibility/statementbuilder)


