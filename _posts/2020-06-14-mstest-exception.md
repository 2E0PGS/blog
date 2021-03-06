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
 - .NET Core
 - .NET Framework
 - .NET
 - C#
---

# MSTest for exception thrown

If you switch between the two frameworks often and use MS test you may find this useful.

## Old MSTest

```
C:\Program Files (x86)\Microsoft Visual Studio\2017\Professional\Common7\IDE\PublicAssemblies\Microsoft.VisualStudio.QualityTools.UnitTestFramework.dll
```

In old .NET Framework test library you don't have the Assert.Throws.

So use below try catch return.

```csharp
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

```
C:\Users\peter\.nuget\packages\mstest.testframework\1.3.2\lib\netstandard1.0\Microsoft.VisualStudio.TestPlatform.TestFramework.dll
```

In new .NET Core test library you can use the following methods.

```csharp
[TestMethod]
public Task GetStuff_ThrowsException_Async()
{
    string foobarString = "testing stuff";
    decimal foobarDecimal = 21;
    await Assert.ThrowsExceptionAsync<Exception>(() => _myController.GetStuffAsync(foobarString, foobarDecimal));
}
```

```csharp
[TestMethod]
public void GetStuff_ThrowsException()
{
    string foobarString = "testing stuff";
    decimal foobarDecimal = 21;
    Assert.ThrowsException<Exception>(() => _myController.GetStuff(foobarString, foobarDecimal));
}
```
