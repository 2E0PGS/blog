---
layout: post
title: MSTest exceptions
date: 2020-06-14 13:28:19
author: Peter Stevenson
summary: MSTest for exception thrown
categories: programming
thumbnail:
tags:
 - MSTest
 - UnitTestFramework
 - dotnet
 - Core
 - Framework
 - .NET
---

# MSTest for exception thrown

If you switch between the two frameworks often and use MS test you may find this useful.

## Old MSTest

`C:\Program Files (x86)\Microsoft Visual Studio\2017\Professional\Common7\IDE\PublicAssemblies\Microsoft.VisualStudio.QualityTools.UnitTestFramework.dll`

In old .NET Framework test library you don't have the Assert.Throws.

So use below try catch return.

```
[TestMethod]
public void GetStuff_ThrowsException()
{
    string foobarString = "testing stuff";
    decimal foobarDecimal = 21;

    try
    {
        var result = _myController.GetStuff(foobarString, foobarDecimal);
    }
    catch (Exception ex)
    {
        return;
    }
    Assert.Fail("Should have thrown an exception.");
}
```

## New MSTest

`C:\Users\peter\.nuget\packages\mstest.testframework\1.3.2\lib\netstandard1.0\Microsoft.VisualStudio.TestPlatform.TestFramework.dll`

In new .NET Core test library you can use the following methods.

`Assert.ThrowsExceptionAsync`

`Assert.ThrowsException`
