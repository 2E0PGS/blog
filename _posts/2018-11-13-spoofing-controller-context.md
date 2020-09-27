---
layout: post
title: Spoofing ASP.NET Framework controller contexts
date: 2018-11-13 12:09:19
author: Peter Stevenson
summary: Rotativa in a ASP.NET Framework Web API
categories: programming
thumbnail:
tags:
 - dotnet
 - aspnet
 - Web
 - API
 - Framework
 - .NET
 - ASP.NET
---

# Spoofing ASP.NET Framework controller contexts

_Moved here from [original gist](https://gist.github.com/2E0PGS/7ef85843464f29aba8335ae204fa56cd)_

This is useful if you need to make use of Rotativa in a ASP.NET Framework Web API instead of a ASP.NET Framework MVC project for example.

## The fake controller context helper class

```
namespace MyProject.Models
{
    public class FakeController : Controller
    {

    }
    public class FakeControllerContextHelper
    {
        public ControllerContext GetFakeControllerContext(string controllerName)
        {
            var context = new HttpContextWrapper(HttpContext.Current);
            var routeData = new RouteData();
            routeData.Values.Add("controller", controllerName);
            var controllerContext = new ControllerContext(new RequestContext(context, routeData), new FakeController());
            return controllerContext;
        }
    }
}
```
## Example of another class making use of our fake controller context helper class

```
// Create new controller instance that you want to spoof.
MagicController magicController = new MagicController();

// Spoof the controller context so Rotativa will believe it's a normal MVC HTML controller and can find the relative views.
Models.FakeControllerContextHelper fakeControllerContexthelper = new Models.FakeControllerContextHelper();
magicController.ControllerContext = fakeControllerContexthelper.GetFakeControllerContext("magic");
```