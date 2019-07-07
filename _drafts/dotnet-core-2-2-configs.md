dotnet core configs

#web 

https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/index?view=aspnetcore-2.2

##injector
https://bitbucket.org/2E0PGS/core/src/master/Core/Startup.cs

# cli
https://stackoverflow.com/questions/38114761/asp-net-core-configuration-for-net-core-console-application

https://stackoverflow.com/a/46437144

# other stuff
Use camelCase

call it appsettings.json
https://stackoverflow.com/a/52341105


```
 "repositories": [
    {
      "projectName": "repo0,
      "repositoryId": "c0476750-050e-47f7-bb55-73a8ce2f4a82"
    },
    {
      "projectName": "repo1",
      "repositoryId": "9f359f34-8c57-454c-bda6-265eacbd031f
"
    },
    {
      "projectName": "repo2",
      "repositoryId": "f2fdc670-e333-45e4-bb5f-b569706c8c7c"
    }
  ]
```

get using

```
var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true);

            IConfigurationRoot configuration = builder.Build();

            GitGitCore.Client.GitGitClient gitGitClient = new Client.GitGitClient("", "");

            var nan = configuration.GetSection("repositories").GetChildren();

            foreach(var repo in configuration.GetSection("repositories").GetChildren())
            {
                string projectName = repo["projectName"];
                string repositoryId = repo["repositoryId"];
            }
```

