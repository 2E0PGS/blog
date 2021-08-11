---
layout: post
title: Portable data layer library
date: 2021-08-11 08:43:19
author: Peter Stevenson
summary: A portable flexible Dapper data layer library
categories: programming
thumbnail:
tags:
 - .NET Standard
 - .NET
 - ASP.NET
 - C#
---

# Portable data layer library

1. Needs to work with dependency injection. E.g. Consumed in large MVC Web application.
2. Needs to work without dependency injection. E.g. Consumed in small console application.
3. Needs to use Dapper ORM. Lightweight, fast and simple.
4. Needs to be portable. We are building a .NET Standard library so why ruin the portability with specific references or tightly coupling to dependencies.
5. I want it to work with SqlConnection, SQLiteConnection and possibly other connectors. Therefore it should be not overly specific.
6. Should be `async` but can also provided non `async` methods.

## Research materials

1. My previous attempts at this.
2. [SQLite + Dapper = Simple Data Access Layer](https://blog.maskalik.com/asp-net/sqlite-simple-database-with-dapper/)
3. [SQLiteDemo](https://github.com/mercury2269/SQLiteDemo/tree/master/SQLiteDemo)
4. [Dapper in ASP.NET Core with Repository Pattern – Detailed](https://www.codewithmukesh.com/blog/dapper-in-aspnet-core/)
5. [Repository Pattern C#](https://codewithshadman.com/repository-pattern-csharp/)

## Research analysed

Numbered points correspond to "Research materials" respectively.

1. When I first started using Dapper I created a simple repository design pattern lib. However it didn't work well for dependency injection and had references to specific dependencies. I wanted to make this truly universal so I can apply it to future applications.
2. Doesn't show namespaces or folder structure so lets look at the actual source. See point 3.
3. Has hard coded file paths. I like the use of a base repository and class inheritance. Has interfaces which is good for DI. Seems excessive to check SQLite file existence on every call. Make use of `using` which is good practice. Folder structure is slightly off.
4. Makes use of `UnitOfWork` pattern which is powerful but potentially overly abstracted. It depends on dependency injection and assumes configuration strings. It demos how to implement the DI which is nice.
5. Very much based around Entity Framework and other large framework design with requirement of DI. Does however give a nice example of using repository pattern from end to end.

## Specific issues

There is some old code which has monolithic design and cross database talking. The new data layer library needs to be a bit flexible around this. So originally there was namespace separation with this hierarchy.

### Namespaces

`MyOrg.SpecialApp.Models.DbOne.CustomerModel.cs`

`MyOrg.SpecialApp.Repositories.DbOne.CustomerRepository.cs`

`MyOrg.SpecialApp.Models.DbTwo.UserModel.cs`

However this makes referencing items messy and solution explorer navigation a mess of expanded folders. For example inside the repository `CustomerRepository` you would have to reference `Models.DbOne.CustomerModel`. Why specify the context which should be default.

Improved is

`MyOrg.SpecialApp.DbOne.Models.CustomerModel.cs`

`MyOrg.SpecialApp.DbOne.Repositories.CustomerRepository.cs`

`MyOrg.SpecialApp.DbTwo.Models.UserModel.cs`

Now inside the repository `CustomerRepository` you can reference `Models.CustomerModel` and all the bits your likely to work on are under one folder too.

I also found it useful to postfix `Repository` to the end of the repository classes so it's immediately obvious they're not a model. Then when intellisense suggests a variable you have a nice `await customerRepository.GetByIdAsync(1);`

## Design notes

* It's all part of engineering.
* It's not uncommon to come up with solutions which work but can be improved upon with hindsight.
* Another thing is business or project requirements change over years.
* We could spend 4 weeks over engineering something simple then come to replace it in years to come or we can implement what looks good in 2 days and replace it if required.
* It could also take many years to find a short coming with a particular design pattern.

## Project examples

* [2E0PGS/user-trak/src/master/UserTrak.Data/](https://bitbucket.org/2E0PGS/user-trak/src/master/UserTrak.Data/) (SQLite)
* [2E0PGS/always-watching-bot/src/master/AlwaysWatchingBot.Data](https://bitbucket.org/2E0PGS/always-watching-bot/src/master/AlwaysWatchingBot.Data/) (SQLite)

### Project example with interfaces for DI scenarios

* TODO: MVC or Web API project example.

### Folder structure example with interfaces

```
|   AlwaysWatchingBot.Data.csproj                 
|                                                 
+---Interfaces                                    
|       IAttachmentRepository.cs                  
|       IMessageRepository.cs                     
|                                                 
+---Models                                        
|       AttachmentModel.cs                        
|       MessageModel.cs                           
|                                                 
\---Repositories                                  
        AttachmentRepository.cs                   
        BaseRepository.cs                         
        MessageRepository.cs.cs                   
```

* One repo per a table at the moment, you can use `UnitOfWork` pattern to combine that into one large object.
* One base repo per a database, normally only one DB per a microservice but if you got monolithic or multiple DBs then you can seperate each DB using namespaces above the three main namespaces.
* TODO: Show multi DB example.
* TODO: Show example interface class for DI as I don't have a public repo with one in.
* TODO: Simple injector example?

## Connection string setup examples

## ASP.NET Framework example with SQL Server connector (Startup.cs)

`Data.Repositories.BaseRepository.ConnectionString = System.Configuration.ConfigurationManager.ConnectionStrings["MyMicroService"].ConnectionString;`

## ASP.NET Core example with SQL Server connector (Startup.cs)

`Data.Repositories.BaseRepository.ConnectionString = Configuration["ConnectionStrings:MyMicroService"];`

## .NET Core Console example with SQLite connector (Program.cs)

`Data.Repositories.BaseRepository.FilePath = _configuration["sqliteFilePath"];`

## DI examples

### ASP.NET Core example (Startup.cs)

`services.AddSingleton<ICustomerRepository, CustomerRepository>();`

### ASP.NET Framework example (Unity Container) (UnityConfig.cs)

`container.RegisterType(typeof(ICustomerRepository), typeof(CustomerRepository));`

or

`container.RegisterType<Data.Interfaces.ICustomerRepository, Data.Repositories.CustomerRepository>();`