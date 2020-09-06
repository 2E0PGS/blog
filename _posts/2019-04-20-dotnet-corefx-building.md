---
layout: post
title: dotnet corefx building
date: 2019-04-20 18:33:19
author: Peter Stevenson
summary: Building corefx for testing a patch
categories: programming
thumbnail:
tags:
 - dotnet
 - aspnet
 - corefx
 - core
 - building
---

# Building corefx for testing a patch

_This post is backdated because I left it sat in my drafts for so long and may not still be relevant. See: [Consolidating .NET GitHub repos #11](https://github.com/dotnet/announcements/issues/119)_ 

A while back I needed to test my corefx patch [dotnet/corefx/pull/36913](https://github.com/dotnet/corefx/pull/36913). Below is the commands I used to build corefx from source and run my program [2E0PGS/program-trak](https://bitbucket.org/2E0PGS/program-trak/src) using the hand compiled corefx runtime. 

`ProcessNameTest` is a simplified version of [2E0PGS/program-trak](https://bitbucket.org/2E0PGS/program-trak/src) I wrote for testing so this name can be interchangeable when trying to understand the below commands.

These commands maybe of use to someone as I recall it was slightly tricky to figure out at the time.

Useful corefx documentation: [advanced-inner-loop-testing.md](https://github.com/dotnet/corefx/blob/master/Documentation/building/advanced-inner-loop-testing.md)

```
cd ~/repos/corefx/

./build.sh -release

rm /home/peter/Documents/ProcessNameTest/ProcessNameTest/runtime/* -rf

cp artifacts/bin/testhost/netcoreapp-Linux-Release-x64/shared/Microsoft.NETCore.App/9.9.9/* /home/peter/Documents/ProcessNameTest/ProcessNameTest/runtime/ -r

cd ~/Documents/ProcessNameTest/ProcessNameTest/runtime/

./corerun /home/peter/repos/corefx/Tools/csc.dll /noconfig /r:System.Runtime.dll /r:System.Runtime.Extensions.dll /r:System.Runtime.InteropServices.dll /r:System.Text.Encoding.Extensions.dll /r:System.Threading.dll /r:System.Linq.dll /r:System.dll /r:System.Reflection.dll /r:System.Diagnostics.Process.dll /r:System.Data.dll /r:System.Private.CoreLib.dll /r:System.Console.dll /r:System.ComponentModel.Primitives.dll /r:System.Collections.NonGeneric.dll  /out:Program.dll ../Program.cs

./corerun Program.dll
```