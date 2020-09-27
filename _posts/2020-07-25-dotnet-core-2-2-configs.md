---
layout: post
title: .NET Core 2.2 configs
date: 2020-07-25 14:17:19
author: Peter Stevenson
summary: Config files in .NET Core 2.2
categories: programming
thumbnail:
tags:
 - dotnet
 - aspnet
 - Web
 - Core
 - config
 - console
 - .NET
 - ASP.NET
 - MVC
---

# Config files in .NET Core 2.2

## .NET Core console application

This doesn't use a dep injector. KISS (Keep It Simple S...): [stackoverflow.com/a/46437144](https://stackoverflow.com/a/46437144)

See real example: [Salsa/Program.cs](https://bitbucket.org/2E0PGS/salsa/src/master/Salsa/Program.cs)

## Note worthy stuff

Use camelCase to conform to JSON standards but it seems MS cant decide if they want to stick to their old school PascalCase days. 

UPDATE: They seem to mostly give camelCase example code snippets now. Originally they had PascalCase JSON property names in the scaffolding templates.

* Call it `appsettings.json`.
* _Untested example migration from web.config: [stackoverflow.com/a/52341105](https://stackoverflow.com/a/52341105)_ I say untested because I believe I did try this and it didn't work however I didn't spend much time at all testing maybe 5 mins.

## ASP.NET Core projects

Dep injection of config: [aspnet/core/fundamentals/configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/index?view=aspnetcore-2.2)

### Setup injector in `Startup.cs` class

```
var builder = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true);

IConfigurationRoot configuration = builder.Build();
```

See real example: [Core/Startup.cs](https://bitbucket.org/2E0PGS/core/src/master/Core/Startup.cs)

### Retrieve/consume configs from a class

```
foreach(var repo in configuration.GetSection("repositories").GetChildren())
{
    string projectName = repo["projectName"];
    string repositoryId = repo["repositoryId"];
}
```

## Example `appsettings.json`

```
"repositories": [
  {
    "projectName": "repo0,
    "repositoryId": "b1821e2e-94c5-4cf0-824c-41d41966207e"
  },
  {
    "projectName": "repo1",
    "repositoryId": "2f6145f9-3472-441d-bb8e-0271b4d82e28"
  },
  {
    "projectName": "repo2",
    "repositoryId": "2b029778-281f-4bb6-82fc-961dfa0f0ad0"
  }
]
```