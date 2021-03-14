---
layout: post
title: .NET Core 2.2 configs
date: 2020-07-25 14:17:19
author: Peter Stevenson
summary: Config files in .NET Core 2.2
categories: programming
thumbnail:
tags:
 - API
 - .NET Core
 - command-line
 - .NET
 - ASP.NET
 - MVC
 - C#
---

# Config files in .NET Core 2.2

## .NET Core console application

This doesn't use a dep injector. KISS (Keep It Simple S...): [stackoverflow.com/a/46437144](https://stackoverflow.com/a/46437144)

Install NuGet package: `Microsoft.Extensions.Configuration.Json`

### Program.cs

```csharp
static void Main(string[] args)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true);

    IConfigurationRoot configuration = builder.Build();

    foreach(var repo in configuration.GetSection("repositories").GetChildren())
    {
        string projectName = repo["projectName"];
        string repositoryId = repo["repositoryId"];
    }
}
```

### Example appsettings.json

Ensure this files properties are set to: "Copy to Output Directory" aka `<CopyToOutputDirectory>`

```json
"repositories": [
  {
    "projectName": "repo0",
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

See real example: [Salsa/Program.cs](https://bitbucket.org/2E0PGS/salsa/src/master/Salsa/Program.cs)

## ASP.NET Core projects

Dep injection of config: [aspnet/core/fundamentals/configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/index?view=aspnetcore-2.2)

### Startup.cs

```csharp
public Startup(IConfiguration configuration)
{
    Configuration = configuration;
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    ...

    services.Configure<MyProject.Models.AppSettings>(Configuration.GetSection("MySection"));
}
```

### Example appsettings.json

```json
{
  "MySection": {
    "ApiUrl": "http://api.myapp.mydomain.org",
    "ApiVersion": 2
  }
}
```

### AppSettings.cs

```csharp
public class AppSettings
{
    public string ApiUrl { get; set; }
    public int ApiVersion { get; set; }
}
```

### Controller.cs

```csharp
public class MyController : Controller
{
    private readonly AppSettings _appSettings;

    public MyController(IOptions<AppSettings> appSettings)
    {
        _appSettings = appSettings.Value;
    }

    public IActionResult Index()
    {
        string apiUrl = _appSettings.ApiUrl;
        return View();
    }
}
```

See real example: [Core/Startup.cs](https://bitbucket.org/2E0PGS/core/src/master/Core/Startup.cs)

### Single string

If you are just accessing a few things in the `Startup.cs` such as [adding auth](https://2e0pgs.github.io/blog/programming/2020/09/27/aspnet-core-2-2-angular-identityserver3/) you can use the following to retrieve one string:

`Configuration.GetValue<string>("Authority");`

```json
{
  "Authority": "https://youridentityserver3instance.com/identity"
}
```

#### Nested single string

If you want to retrieve a nested single string you can do the following. Notice how here we are using a index style access `[]` instead of the `GetValue` or `GetSection` methods. This is yet another way to retrieve config strings. If you used XML `Web.config` in .NET Framework this will be more familiar.

`Configuration["ConnectionStrings:MyDatabase"];`

```json
{
  "ConnectionStrings": {
    "MyDatabase": "Data Source=localhost;Initial Catalog=MyDatabase;Integrated Security=True"
  }
}
```

## Note worthy stuff

Use camelCase to conform to JSON standards but it seems MS cant decide if they want to stick to their old school PascalCase days. 

UPDATE: They seem to mostly give camelCase example code snippets now. Originally they had PascalCase JSON property names in the scaffolding templates.

* Call it: `appsettings.json`
* _Untested example migration from web.config: [stackoverflow.com/a/52341105](https://stackoverflow.com/a/52341105)_ I say untested because I believe I did try this and it didn't work however I didn't spend much time at all testing maybe 5 mins.
