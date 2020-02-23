

## Using Git Submodules in dotnet framework

You will need to migrate all the projects involved away from `packages.config` to `project.json`. See below headings.

TODO: Check up on doetnet framework 4.6.5 projects using `Project.csproj` package references.

You will need to clone the Git Submodule into a folder. I use the solutions root folder `/lib`.

You can then reference the Submodule project relative to the solution. Ensure it's relative path otherwise it wont work on other peoples machines.

## Related blog posts

* https://blogs.ancestry.com/ancestry/2015/02/26/lesson-learned-sharing-code-with-git-submodule/

* https://github.com/saturn72/SolutionWithGitSubmodulesAndNuget

## Migrating to project.json

Official documentation on the process from a more manual approach: [Converting a csproj from package.config to project.json
](https://github.com/NuGet/Home/wiki/Converting-a-csproj-from-package.config-to-project.json)

A related blog post on the topic: [Oren Codes - Project.json all the things](https://oren.codes/2016/02/08/project-json-all-the-things/)

Using the powershell script: [nugetprojectjson](https://github.com/wgtmpeters/nugetprojectjson)3

Example for a dotnet framework 4.6.5 project: `.\create_project_json.ps1 -r -f net461`

A couple fixes and issues I found for you to be aware of:

[nugetprojectjson/issues/5](https://github.com/wgtmpeters/nugetprojectjson/issues/5)

[nugetprojectjson/pull/4](https://github.com/wgtmpeters/nugetprojectjson/pull/4)

## Dotnet standard 2.0 and framework 4.6.1

If you plan to consume a dotnet standard 2.0 library in a dotnet framework 4.6.1 project you will need Visual Studio 2017 or newer.

If it's a MVC web project you will need to run the project, stop it and then double click the binding redirect warning to update the `Web.Config` automatically with the correct bindings.

## Potential roadblocks

* [NuGet hates Git submodules that use NuGet](https://github.com/NuGet/Home/issues/4124#issuecomment-269487836)
* You may find VS intellisense gets confused sometimes.
