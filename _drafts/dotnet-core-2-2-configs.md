header....

# dotnet core configs

## CLI projects

This doesn't use a dep injector. KISS (Keep It Simple S...): https://stackoverflow.com/a/46437144

See real example: [Salsa/Program.cs](https://bitbucket.org/2E0PGS/salsa/src/master/Salsa/Program.cs)

## Note worthy stuff

Use camelCase to conform to JSON standards but it seems MS cant decide if they want to stick to their old school PascalCase days. 

UPDATE: They seem to mostly give camelCase example code snippets now. Originally they had PascalCase JSON property names in the scaffolding templates.

* Call it `appsettings.json`.
* ??? Example migration from web.config: https://stackoverflow.com/a/52341105

## aspnet web projects

Dep injection of config: https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/index?view=aspnetcore-2.2

### Setup injector in `Startup.cs` class

```
var builder = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true);

IConfigurationRoot configuration = builder.Build();
```

See real example: [Core/Startup.cs](https://bitbucket.org/2E0PGS/core/src/master/Core/Startup.cs)

### Retrive/consume configs from a class

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
    "repositoryId": "c0476750-050e-47f7-bb55-73a8ce2f4a82"
  },
  {
    "projectName": "repo1",
    "repositoryId": "9f359f34-8c57-454c-bda6-265eacbd031f"
  },
  {
    "projectName": "repo2",
    "repositoryId": "f2fdc670-e333-45e4-bb5f-b569706c8c7c"
  }
]
```