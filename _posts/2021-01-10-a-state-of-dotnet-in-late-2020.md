---
layout: post
title: A state of .NET in late 2020
date: 2021-01-10 19:42:19
author: Peter Stevenson
summary: A snapshot in time of the current stack
categories: programming
thumbnail:
tags:
 - .NET Framework
 - .NET Core
 - .NET Standard
 - .NET
 - ASP.NET
 - C#
---

# A state of .NET in late 2020

A hub of links and comments which cover most of the realization of .NET Core. Acting as a snapshot in time of the current stack. 

Hopefully also highlights some issues which can aid in predicting potential future concerns going forwards with various new technologies. 

Otherwise useful as a historic reference for frameworks built on these stacks.

## Razor pages

* [Razor pages ASP.NET Core 3.1](https://docs.microsoft.com/en-us/aspnet/core/razor-pages/?view=aspnetcore-3.1)
* [Razor pages ASP.NET Core 2.2](https://docs.microsoft.com/en-us/aspnet/core/razor-pages/?view=aspnetcore-2.2&tabs=visual-studio)
* `asp-for` instead of `@Html.` helpers. See: [What is the advantage of using Tag Helpers in ASP.NET Core MVC](https://stackoverflow.com/questions/29005877/what-is-the-advantage-of-using-tag-helpers-in-asp-net-core-mvc)
* Keeps the code closer to the view for better organisation.

[ASP.NET Razor Pages vs MVC: How Do Razor Pages Fit in Your Toolbox?](https://stackify.com/asp-net-razor-pages-vs-mvc/)

![](https://stackify.com/wp-content/uploads/2017/08/img_5994b41da1992.png)

## Blazor

* [Blazor Build client web apps with C#](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor)
* WebAssembly and server side rendering mode although the WebAssembly stuff doesn't seem to be pushed as much yet.
* Baked in SignalR which is useful for that alone.
* [dotNET South West - What is Blazor? And why’s it so exciting? with Chris Sainty, April 2020](https://youtu.be/Es2HUWCQQts)

## Razor/Blazor class libs

* Useful for sharing partial views across services. E.g. Login form controls.
* Common error page and privacy page razor views.
* [Create reusable UI using the Razor class library project in ASP.NET Core 3.1](https://docs.microsoft.com/en-us/aspnet/core/razor-pages/ui-class?view=aspnetcore-3.1&tabs=visual-studio)
* [Razor class library and reusable blazor components](https://www.youtube.com/watch?v=BtXWj-yQF6c)
* Targets `.NET Standard 2.0` so can work across .NET Framework and .NET Core.

## WinForms

* [Windows Forms](https://github.com/dotnet/winforms)
* Still relevant for Windows native .NET Framework programs and Microsoft recently added support for .NET Core WinForms.

## WebForms

* A bit like WinForms but for the web.
* Deprecated for a while now in favour of MVC.
* Wont be in .NET 5 as replaced by Blazor.

## WPF

* TODO: Deprecated? Was once promoted above WinForms?

## UWP

* [Microsoft Confirms UWP is Not the Future of Windows Apps](https://www.thurrott.com/dev/206351/microsoft-confirms-uwp-is-not-the-future-of-windows-apps)
* 

## MVC

* [Overview of ASP.NET Core MVC](https://docs.microsoft.com/en-us/aspnet/core/mvc/overview?view=aspnetcore-3.1)
* [Getting started with ASP.NET MVC 5 .NET Framework 4.6.1](https://docs.microsoft.com/en-us/aspnet/mvc/overview/getting-started/introduction/getting-started)
* [I'm lost. What happened to ASP.NET MVC 5?](https://stackoverflow.com/a/51391202)
	* Do not confuse **ASP.NET Framework 4.x.x MVC** or **ASP.NET 5 MVC** with **MVC 5** or **ASP.NET MVC 5**.
	* See the following table which shows MVC version supported by .NET Framework version: [ASP.NET MVC Version History](https://www.tutorialsteacher.com/mvc/asp.net-mvc-version-history)
	* [wikipedia.org/wiki/ASP.NET_MVC](https://en.wikipedia.org/wiki/ASP.NET_MVC)
		* \> The ASP.NET MVC is a not actively developed web application framework developed by Microsoft
		* \> ASP.NET Core has since been released, which unified ASP.NET, ASP.NET MVC, ASP.NET Web API, and ASP.NET Web Pages (a platform using only Razor pages). MVC 6 was abandoned due to Core and is not expected to be released. Core is currently planned to merge into ".NET 5".
		* See the release history section for versions.
	* [wikipedia.org/wiki/ASP.NET](https://en.wikipedia.org/wiki/ASP.NET)		
		* \> The ASP.NET releases history tightly correlates with the .NET Framework releases
		* See the versions section.

### Areas

* [Areas in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/areas?view=aspnetcore-3.1)
* Useful for organising MVC projects where you want to keep Models, Views and Controllers isolated.
* See real example: [2E0PGS - Core/Areas](https://bitbucket.org/2E0PGS/core/src/master/Core/Areas/)

#### Namespaces

* TODO: My blog post on: [TBA - Portable data layer library - Namespaces]()
* Better namespaces for easier and shorter references as the Controllers can reference Models within same namespace.

`MyOrg.SpecialApp.Models.System1.CustomerModel.cs`

`MyOrg.SpecialApp.Controllers.System1.CustomerController.cs`

Reference example: `using Models.System1.CustomerModel.cs`

vs

`MyOrg.SpecialApp.Areas.System1.Models.CustomerModel.cs`

`MyOrg.SpecialApp.Areas.System1.Controllers.CustomerController.cs`

Reference example: `using Models.CustomerModel.cs`

## .NET Core with Angular/React

* Allows you to create a solution which comes with a .NET Core as the backend API and Angular or React as the SPA.
* Not useful if you already have an established API or you don't want the confusion of building/running Angular/React inside a VS project/solution.
* TypeScript is great and required imo for such complex JS.

## Cross platform UI

* [avalonia](https://avaloniaui.net/)
* [Introducing .NET Multi-platform App UI](https://devblogs.microsoft.com/dotnet/introducing-net-multi-platform-app-ui/)
	* \> .NET MAUI is an evolution of the increasingly popular Xamarin.Forms toolkit
* [github.com/dotnet/maui](https://github.com/dotnet/maui)
* [Electron.NET](https://github.com/ElectronNET/Electron.NET)
* [Xamarin.Forms](https://github.com/xamarin/Xamarin.Forms) single shared UI layer instead of one for each platform like Xamarin traditional. However you can still implement native APIs if required.
* [Uno Platform](https://github.com/unoplatform/uno)

## Some consolidation works already and abstractions

* [wikipedia.org/wiki/ASP.NET_Core](https://en.wikipedia.org/wiki/ASP.NET_Core)
	* \> The framework is a complete rewrite that unites the previously separate ASP.NET MVC and ASP.NET Web API into a single programming model.
* The controllers are the same for both Web API and MVC projects.
* ASP.NET Core `IActionResult` instead of `HttpActionResult` ASP.NET Framework MVC or `IHttpActionResult` ASP.NET Framework Web API
* Tool for migrating from .NET Framework to .NET Core: [try-convert](https://github.com/dotnet/try-convert)
* [Consolidating .NET GitHub repos](https://github.com/dotnet/announcements/issues/119)

## Overview of framework topology

```
ASP.NET Framework Web Forms
ASP.NET Framework MVC
ASP.NET Framework Web API
ASP.NET Core Blazor
ASP.NET Core Pages 
ASP.NET Core MVC
ASP.NET Core Web API

ASP.NET Framework
ASP.NET Core

.NET Framework
.NET Core

.NET Standard

Language e.g. C#

MSIL

CLR
```

## Overview of build system topology

### .NET Framework

```
MSBuild -> Roslyn -> csc.exe
```

### .NET Core

```
.NET Core CLI -> MSBuild -> Roslyn -> csc.dll
```

### Linux

#### strace 

`strace -o strace.txt -f -t -e trace=file dotnet build`

`/usr/share/dotnet/sdk/2.2.402/MSBuild.dll`

`/usr/share/dotnet/sdk/2.2.402/Roslyn/bincore/csc.dll`

#### Verbose build

`dotnet build --verbosity normal`

```
CoreCompile:
	/usr/share/dotnet/dotnet exec "/usr/share/dotnet/sdk/2.2.402/Roslyn/bincore/csc.dll"
```

> and the newest 2.2.2 SDK ships with csc.dll and vbc.dll instead of csc.exe and vbc.exe

Ref: [https://stackoverflow.com/a/47697996](https://stackoverflow.com/a/47697996)

## Project/package management

* Various project management methods: `package.config`/`packages.config`, `Project.csproj`, `project.json`
* PR I opened: [Replace typo package.config with packages.config](https://github.com/NuGet/docs.microsoft.com-nuget/pull/2180)
* My blog post on: [Submodules in .NET](https://2e0pgs.github.io/blog/programming/2020/02/23/submodules-in-dotnet/) specifically around some of the quirks and teething problems with NuGet and framework interoperability.

## Project structure

I personally stick with the way .NET Framework projects where historically laid out. Fredrik's below blog post shows how Dapper is similar to that. There does however seem to be a movement towards src/test/docs convention for some Open Source .NET projects.

* [Fredrik Erasmus - Organizing .NET Core Projects](https://medium.com/@fredrik_erasmus/organizing-net-core-projects-7ee335ea6af)
* [David Fowler - .NET project structure](https://gist.github.com/davidfowl/ed7564297c61fe9ab814)

## Configuration management

* My blog post on: [.NET Core 2.2 configs](https://2e0pgs.github.io/blog/programming/2020/07/25/dotnet-core-2-2-configs/)
* Settings.settings: [Using Application Settings and User Settings](https://docs.microsoft.com/en-us/dotnet/desktop/winforms/advanced/using-application-settings-and-user-settings?view=netframeworkdesktop-4.8)
* Settings.Default: [How To: Read Settings at Run Time With C#](https://docs.microsoft.com/en-us/dotnet/desktop/winforms/advanced/how-to-read-settings-at-run-time-with-csharp?view=netframeworkdesktop-4.8)

## Unit testing

* My blog post on: [MSTest for exception thrown](https://2e0pgs.github.io/blog/programming/2020/06/14/mstest-exception/)  in .NET Core and .NET Framework.

## Service reference/Web service/WCF

* "Web Reference" - .NET Framework
* "Service Reference" - .NET Framework
* "WCF Web Service Reference Provider" - .NET Core and .NET Standard
	* [Use the WCF Web Service Reference Provider Tool](https://docs.microsoft.com/en-us/dotnet/core/additional-tools/wcf-web-service-reference-guide)
	* [WCF Web Service Reference Provider - Release Notes](https://github.com/dotnet/wcf/blob/master/release-notes/WCF-Web-Service-Reference-notes.md)

## IDEs

* VS Code, Visual Studio and SMMS.
* I am mostly using a Visual Studio 2017, SMSS 2017 and VS Code workflow at the moment. This matches with my C# version and framework version choices.
* I noticed many are shifting to light weight IDEs. See my blog post: [A state of text editors in late 2019](https://2e0pgs.github.io/blog/programming/2019/12/07/a-state-of-text-editors-in-late-2019/)
* Many extensions have been created for VS Code which allow interaction with databases etc. Will these replace tools like SMSS eventually?
* TODO: My blog post on: [TBA - virtualised programming]() specifically around live share?
* [Visual Studio version table](https://en.wikipedia.org/wiki/Microsoft_Visual_Studio#History)

### VS Code and .NET Framework

* I prefer using Visual Studio running in Windows still.
* [omnisharp-vscode - Desktop .NET Framework](https://github.com/OmniSharp/omnisharp-vscode/wiki/Desktop-.NET-Framework)
* [Visual Studio Code for .Net Framework](https://stackoverflow.com/questions/47707095/visual-studio-code-for-net-framework)
* [Using Visual Studio Code for .NET Framework Projects in C#](https://www.medo64.com/2019/04/using-visual-studio-code-for-net-framework-projects-in-c-sharp)

#### Windows

* Seems to work reasonable on Windows for viewing, occasionally struggles to find or peek references.
* TODO: `dotnet run` on a .NET Framework console app?

```
[info]: OmniSharp.MSBuild.Discovery.MSBuildLocator
	Located 2 MSBuild instance(s)
		1: Visual Studio Professional 2017 15.x.xxxxx.xxx s- "C:\Program Files (x86)\Microsoft Visual Studio\2017\Professional\MSBuild\15.0\Bin"
		2: StandAlone 16.4 - "C:\Users\peter\.vscode\extensions\ms-dotnettools.csharp-1.21.18\.omnisharp\1.35.1\.msbuild\Current\Bin"
```

#### Linux

* Doesn't work well if using Linux, probably because it's missing the tooling/SDK for intellisense to work.

```
[info]: OmniSharp.MSBuild.Discovery.MSBuildLocator
        Located 1 MSBuild instance(s)
            1: StandAlone 16.4 - "/home/peter/.vscode/extensions/ms-dotnettools.csharp-1.21.18/.omnisharp/1.35.1/omnisharp/.msbuild/Current/Bin"
...
[fail]: OmniSharp.MSBuild.ProjectLoader
        The reference assemblies for .NETFramework,Version=v4.6.1 were not found. To resolve this, install the Developer Pack (SDK/Targeting Pack) for this framework version or retarget your application. You can download .NET Framework Developer Packs at https://aka.ms/msbuild/developerpacks
```

### VS Code and .NET Core

* Works fantastic on Windows and Linux now however when it first came out with support for .NET Core it had barely any intellisense and was unusable imo.

### VS Code and .NET Standard

* Works fantastic on Windows and Linux.

### Visual Studio 2017

* Works great for C# 7, .NET Framework 4.6.1, .NET Core 2.1, .NET Core 2.2 and .NET Standard 2.0
* [Announcing F# support for .NET Core and .NET Standard projects in Visual Studio](https://devblogs.microsoft.com/dotnet/announcing-f-support-for-net-core-and-net-standard-projects-in-visual-studio/)

### Visual Studio 2019

* Current latest.

## Language versioning

[C# language versioning](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/configure-language-version)

> The rules in this article apply to the compiler delivered with Visual Studio 2019 or the .NET SDK. The C# compilers that are part of the Visual Studio 2017 installation or earlier .NET Core SDK versions target C# 7.0 by default.

You can override target language version in the Project.csproj:

```xml
<LangVersion>7.1</LangVersion>
```

Otherwise using the compiler option: `-langversion:7.1`

### C# 7

* [What's new in C# 7.0 through C# 7.3](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-7)
* C# 7.1 allows for [Async main](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-7#async-main)

### C# 8

* [What's new in C# 8.0](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-8)
* [Using declarations](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-8#using-declarations)

### C# 9

* [What's new in C# 9.0](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-9)
* [Microsoft Developer YouTube Channel - Future of C#](https://youtu.be/kU-t1qsEbqk)
* I believe it requires VS 2019.
* The language is becoming less "belts and braces", looking like you can use C# in more of a scripting way with less overhead of entry method declarations. 
* `record` and `with` expressions to mutate objects safely. A bit like shallow copying.
* Method params creating members automatically.
* [New C#9 keywords ‘and’ ‘or’ ‘not’](https://blog.ndepend.com/csharp9-new-keywords-for-pattern-matching-and-or-not/) could be confused with T-SQL, I have seen other developers put T-SQL syntax/logical operators into C# by mistake.
* Similar things mentioned in: [.NET Conf 2020](#net-conf-2020)

## Interactive

* [Try .NET](https://github.com/dotnet/try)
* [.NET In-Browser](https://dotnet.microsoft.com/learn/dotnet/in-browser-tutorial/1)
* My blog post: [REPL maths and notation](https://2e0pgs.github.io/blog/programming/2020/09/27/repl-maths-and-notation/)
* Rather hard to find this package. Seems to be Microsoft's version of Jupyter notebooks. It's aka ".NET notebooks" or "native notebook" [.NET Interactive Notebooks](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.dotnet-interactive-vscode) also see: [GitHub Issue Notebook](https://code.visualstudio.com/updates/v1_45#_github-issue-notebook)
* One of the only videos I have seen on ".NET Interactive Notebooks": [F# as a Better Python - Phillip Carter - NDC Oslo 2020](https://youtu.be/_QnbV6CAWXc?t=2367) besides a mention By Phillip in [.NET Conf 2020](#net-conf-2020)
* There is also regular Jupyter file support using a "kernel" in other words a Jupyter server: [Working with Jupyter Notebooks in Visual Studio Code](https://code.visualstudio.com/docs/python/jupyter-support)
* [.csx files - Essential .NET C# Scripting](https://docs.microsoft.com/en-us/archive/msdn-magazine/2016/january/essential-net-csharp-scripting)

## Frameworks

* [Target frameworks in SDK-style projects](https://docs.microsoft.com/en-us/dotnet/standard/frameworks)

### .NET Core and .NET Framework

Because most of this document covers these two frameworks, this heading will only cover bits that don't fit elsewhere.

* [.NET Core is the Future of .NET ](https://devblogs.microsoft.com/dotnet/net-core-is-the-future-of-net/)
	* \> New applications should be built on .NET Core. .NET Core is where future investments in .NET will happen. Existing applications are safe to remain on .NET Framework which will be supported.
* TODO: Session, TempData, Viewbag (evil Viewbags)? 
	* TODO: One day explain some MVC recommendations and why to avoid Viewbags or hidden fields for certain things.
* TODO: Manually serialise TempData due to different in memory backing. I use Newtonsoft.
	* [Complex Objects and TempData using .NET Core 2](https://dotnetevolved.com/2017/08/complex-objects-and-tempdata-using-net-core-2/)

### .NET 5

* This mostly covers new features in .NET 5 however some features already released or not directly related maybe mentioned under other headings.
* And now .NET 5 is unifying everything, I guess MS realised things could be getting a bit "bitty". However let's hope the package management, project/solution management, build tools and such can be unified.
* [IAmTimCorey - What is the Future of .NET? Is .NET Framework Dead? Is .NET Core Dead?](https://www.youtube.com/watch?v=ZwxWCiW5uO4)
* [Introducing .NET 5 | .NET Blog](https://devblogs.microsoft.com/dotnet/introducing-net-5/)

Some interesting points from the .NET blog post include:

* The ability to swap CoreCLR and Mono runtimes interchangeably via build flags.
* The names ".NET Core 5" and ".NET 5" can be used interchangeably however they dropped the "Core" to show it's a unifying framework.
* .NET 5 is the future for the .NET platform.
* "Ahead Of Time" native compilation added for certain workloads. Previously only "Just In Time" compilation.
* Also see the below diagram which shows the stack along with tooling. Notice CLI is on there and a big part of .NET now.

![](https://devblogs.microsoft.com/dotnet/wp-content/uploads/sites/10/2019/05/dotnet5_platform.png)

#### .NET Conf 2020

Some interesting points from [.NET Conf 2020](https://www.dotnetconf.net/) include:

* OpenAPI/Swagger support in .NET 5 Web API projects on-by-default.
* More use of REPL e.g. `httprepl.exe` If you don't know what a REPL is see heading: [Interactive](#interactive)
* Platform Compatibility Analyzer included/on-by-default in new projects.
* Plans for Blazor on the desktop.
* New .NET releases every year.
* Protected browser storage. Is that new?
* Xamarin not merged till .NET 6?
* Linux debugging with VS on Windows and WSL 2 or remote Linux debugging over SSH.
* `dotnet dump` for memory dumps on .NET Core, dumps can be analysed on other platforms.
* EF Core with tooling. Lots of providers: https://docs.microsoft.com/en-us/ef/core/providers/?tabs=dotnet-core-cli
* The new [YARP](https://github.com/microsoft/reverse-proxy) reverse proxy middleware/toolkit written in C# with good extensibility. Relates to IIS ARR (see my blog post: [IIS reverse proxy server](https://2e0pgs.github.io/blog/sysadmin/2020/06/04/iis-reverse-proxy/)) and [Kestrel](https://github.com/aspnet/KestrelHttpServer). Will this replace NGINX reverse proxy for many use cases?
* [Bringing .NET Interactive to Azure Data Studio Notebooks](https://youtu.be/938jBJ-tK3c)

#### .NET Standard

* [The future of .NET Standard | .NET Blog](https://devblogs.microsoft.com/dotnet/the-future-of-net-standard/)
* Continue using .NET Standard 2.0+ for portable libraries across .NET Framework and .NET Core.
* Going forwards when projects are migrated to .NET 5 there will be no need for .NET Standard.

## Platform specific APIs

* A issue I opened: [OS compatibility/portability information](https://github.com/NuGet/NuGetGallery/issues/7079)
* Second issue I opened: [NuGet Gallery platform specific API information](https://github.com/dotnet/platform-compat/issues/229)
* Useful NuGet package for analysis: [Platform Compatibility Analyzer](https://github.com/dotnet/platform-compat)
* [The .NET Portability Analyzer](https://docs.microsoft.com/en-us/dotnet/standard/analyzers/portability-analyzer)

## Blueprints/templates/scaffolding

* Shame all 3 words get thrown around a bit. Could be a more consolidated term.
* Anyway I noticed these have changed a fair bit for .NET Core projects. Specifically for MVC projects.
* `dotnet new` is for blueprints in .NET Core. You can get extra ones online for example: [AWS blueprints](https://github.com/aws/aws-lambda-dotnet/blob/master/Blueprints/README.md)

### Example

```
$ dotnet --version
2.2.402

$ dotnet new
...
Templates                                         Short Name         Language          Tags                                 
----------------------------------------------------------------------------------------------------------------------------
Console Application                               console            [C#], F#, VB      Common/Console                       
Class library                                     classlib           [C#], F#, VB      Common/Library                       
Unit Test Project                                 mstest             [C#], F#, VB      Test/MSTest                          
NUnit 3 Test Project                              nunit              [C#], F#, VB      Test/NUnit                           
NUnit 3 Test Item                                 nunit-test         [C#], F#, VB      Test/NUnit                           
xUnit Test Project                                xunit              [C#], F#, VB      Test/xUnit                           
Razor Page                                        page               [C#]              Web/ASP.NET                          
MVC ViewImports                                   viewimports        [C#]              Web/ASP.NET                          
MVC ViewStart                                     viewstart          [C#]              Web/ASP.NET                          
ASP.NET Core Empty                                web                [C#], F#          Web/Empty                            
ASP.NET Core Web App (Model-View-Controller)      mvc                [C#], F#          Web/MVC                              
ASP.NET Core Web App                              webapp             [C#]              Web/MVC/Razor Pages                  
ASP.NET Core with Angular                         angular            [C#]              Web/MVC/SPA                          
ASP.NET Core with React.js                        react              [C#]              Web/MVC/SPA                          
ASP.NET Core with React.js and Redux              reactredux         [C#]              Web/MVC/SPA                          
Razor Class Library                               razorclasslib      [C#]              Web/Razor/Library/Razor Class Library
ASP.NET Core Web API                              webapi             [C#], F#          Web/WebAPI                           
global.json file                                  globaljson                           Config                               
NuGet Config                                      nugetconfig                          Config                               
Web Config                                        webconfig                            Config                               
Solution File                                     sln                                  Solution
```

## Cloud

* AWS, see my blog post: [ASP.NET Core Web API AWS Lambda](https://2e0pgs.github.io/blog/programming/2020/10/01/aspnet-core-web-api-aws-lambda/)
* TODO: Azure

## WebHooks

* See my blog post: [ASP.NET WebHooks](https://2e0pgs.github.io/blog/programming/2020/07/25/aspnet-webhooks/)

## Diagrams

Some useful topology diagrams of the current stack.

[.NET Core, .NET Framework, Xamarin – The “WHAT and WHEN to use it”](https://devblogs.microsoft.com/cesardelatorre/net-core-1-0-net-framework-xamarin-the-whatand-when-to-use-it/)

![](https://devblogs.microsoft.com/wp-content/uploads/sites/32/2016/06/image_thumb751.png)

[Cross-platform targeting](https://docs.microsoft.com/en-us/dotnet/standard/library-guidance/cross-platform-targeting)

![](https://docs.microsoft.com/en-us/dotnet/standard/library-guidance/media/cross-platform-targeting/platforms-netstandard.png)

[ASP.NET Core 1.0. Part 1: Introduction, general description and the future of .NET Framework](https://codingsight.com/asp-net-core-1-0-part-1-introduction-general-description-future-net-framework/)

![](https://codingsight.com/wp-content/uploads/2016/12/NET-Framework-Today-En-1024x675.jpg)

[ASP.NET vNext - DevExpress Plans for ASP.NET 5](https://community.devexpress.com/blogs/aspnet/archive/2015/08/04/asp-net-vnext-devexpress-plans-for-asp-net-5.aspx)

![](https://community.devexpress.com/blogs/aspnet/15.1Release/Net10kView1.png)

[.NET Core CLI msbuild architecture](https://docs.microsoft.com/en-us/dotnet/core/tools/cli-msbuild-architecture)

![](https://docs.microsoft.com/en-us/dotnet/core/tools/media/cli-msbuild-architecture/p2-arch.png)

[Uncovering the cross-platform capabilities of .NET](https://headspring.com/2018/07/10/cross-platform-capabilities-of-dot-net/)

![](https://headspring.com/wp-content/uploads/2018/07/Dot-Net-cross-platform-diagram.png)

[C# 7.1 and .NET Core 2.0 - Modern Cross-Platform Development - Third Edition](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.packtpub.com%2Fproduct%2Fc-7-1-and-net-core-2-0-modern-cross-platform-development-third-edition%2F9781788398077&psig=AOvVaw2Kocn6_RluzVIioBx_BS-P&ust=1601222213014000&source=images&cd=vfe&ved=0CAkQjhxqFwoTCPiIy92Xh-wCFQAAAAAdAAAAABAZ)

![](https://static.packt-cdn.com/products/9781788398077/graphics/395440c3-7a7d-41f3-a4b9-39a561c85c2f.png)

[execution process of .NET Core and compare it with the .NET Framework.](https://www.tutorialspoint.com/dotnet_core/dotnet_core_code_execution.htm)

![](https://www.tutorialspoint.com/dotnet_core/images/code_execution.jpg)

![](https://www.tutorialspoint.com/dotnet_core/images/dotnet_core_code_execution.jpg)