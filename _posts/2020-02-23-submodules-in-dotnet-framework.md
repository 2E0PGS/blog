---
layout: post
title: Submodules in dotnet framework
date: 2020-02-23 17:16:10
author: Peter Stevenson
summary: Using Git Submodules in dotnet framework
categories: programming
thumbnail:
tags:
 - Git
 - Submodules
 - dotnet
 - core
 - standard
 - framework
 - NuGet
---

# Using Git Submodules in dotnet framework

You will need to migrate all the projects involved away from `packages.config` to `project.json`. See below project type specific instructions.

You will need to clone the Git Submodules into a folder. I use the solutions root folder `/lib`. See: [core/src/develop/lib/](https://bitbucket.org/2E0PGS/core/src/develop/lib)

You can then reference the Submodule project relative to the solution. Ensure it's relative path otherwise it wont work on other peoples machines.

## How to migrate package.config to project.json

If I recall correctly this is due to NuGet deps were previously restored per a project instead of per a solution.

Official wiki page but a more manual approach: [Converting a csproj from package.config to project.json](https://github.com/NuGet/Home/wiki/Converting-a-csproj-from-package.config-to-project.json)

A related blog post on the topic: [Oren Codes - Project.json all the things](https://oren.codes/2016/02/08/project-json-all-the-things/)

Powershell script to automate it: [nugetprojectjson](https://github.com/wgtmpeters/nugetprojectjson)

Example command for a dotnet framework 4.6.1 project: `.\create_project_json.ps1 -r -f net461`

A couple fixes and issues I found for you to be aware of:

[nugetprojectjson/issues/5](https://github.com/wgtmpeters/nugetprojectjson/issues/5)

[nugetprojectjson/pull/4](https://github.com/wgtmpeters/nugetprojectjson/pull/4)

## Project types

### Dotnet standard 2.0 class library and dotnet framework 4.6.1 MVC web

This assumes a dotnet standard 2.0 class library with `Project.csproj` package references and a dotnet framework 4.6.1 MVC web project with `package.config`.

1. Migrate the dotnet framework project to project.json.
2. If you plan to consume a dotnet standard 2.0 library in a dotnet framework 4.6.1 project you will need Visual Studio 2017 or newer.
3. If it's a MVC web project you will need to run the project, stop it and then double click the binding redirect warning to update the `Web.Config` automatically with the correct bindings.

### Dotnet framework 4.6.1 class library and dotnet framework 4.6.1 MVC web

This assumes two dotnet framework 4.6.1 projects with `package.config`.

1. Migrate both projects to project.json.
2. You wont need binding redirects unless you have a version mismatch in NuGet deps.

### Dotnet framework 4.6.1 PackageReference format

* Material on dotnet framework 4.6.1 and package reference not supported see: [NuGet/Home/issues/6763](https://github.com/NuGet/Home/issues/6763)
* [Migrate packages.config to PackageReference](https://docs.microsoft.com/en-us/nuget/consume-packages/migrate-packages-config-to-package-reference#limitations)

### Dotnet standard 2.0 class library and dotnet core web 2.0+

This assumes a dotnet standard 2.0 class library and a dotnet core web project with `Project.csproj`.

Works fine without all this mumbo jumbo. See example: [Core/Core.csproj](https://bitbucket.org/2E0PGS/core/src/develop/Core/Core.csproj)

## Framework compatibility

Some of these issues are around dotnet framework 4.6.1 not being "fully" compatible with dotnet standard 2.0. Despite the [.NET implementation support](https://docs.microsoft.com/en-us/dotnet/standard/net-standard#net-implementation-support) suggesting otherwise. Dotnet framework 4.7.2 is supposed to be fully supported.

See point 2 under the [.NET implementation support](https://docs.microsoft.com/en-us/dotnet/standard/net-standard#net-implementation-support) table for more information.

This is more to do with the binding redirects and versions of dependencies as dotnet framework 4.6.1 actually "implements .NET Standard 1.4". However it will support up to dotnet standard 2.0 with binding redirects.

## Azure DevOps/VSTS CI

You will need to ensure clone Submodules is part of the continues integration pipeline.

## Potential roadblocks

* [NuGet hates Git Submodules that use NuGet](https://github.com/NuGet/Home/issues/4124#issuecomment-269487836)
* You may find VS intellisense gets confused sometimes.

## Other links

* [Ancestry - Lesson Learned Sharing Code With Git Submodules](https://blogs.ancestry.com/ancestry/2015/02/26/lesson-learned-sharing-code-with-git-submodule/)
* [Example solution with Git Submodules and NuGet](https://github.com/saturn72/SolutionWithGitSubmodulesAndNuget)