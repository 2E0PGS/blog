---
layout: post
title: REPL maths and notation
date: 2020-09-27 11:02:10
author: Peter Stevenson
summary: Maths, notation, calculators and REPL programming
categories: programming
thumbnail:
tags:
 - Maths
 - C#
 - SQL
 - .NET
---

# Maths, notation, calculators and REPL programming

## Notation

* Prefix notation also called polish notation. Example: `+ 5 5`
* Postfix notation also called reverse polish notation. Example: `5 5 +`
* Infix notation, this is what most people use daily. Example: `5 + 5`

Algebra...? is this a notation or expression or convention or a daft question? Seems it's classed as a language of its own.

Operators and operands.

Buffers in polish notation are like computer registers/buffer.

## Calculators

#### fx-991ES PLUS

This Casio calculator is set by default to give fractional output. You can change it to decimal output using the below commands. [Reference](https://www.quora.com/My-calculator-is-giving-me-answers-in-fractions-when-using-decimals-and-division-How-do-I-change-it-back-to-giving-me-answers-in-whole-numbers-and-decimals)

```
Go to [Shift ] [Setup]

Hit [Math IO]

Select [Line O]
```

Last output is stored in variable: `Ans`

Old Windows XP calculator emulator programs. 

HP PDA Windows CE scientific calculator programs. 

## REPL and maths in programming languages

[REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) environment as a calculator.

You will run into int based calculations unless specifying decimal variables/input for division etc.

### C#

* [Floating point math in C#](https://0.30000000000000004.com/#csharp)
* [Learning C# 3.0 - Mathematical Operators](https://www.oreilly.com/library/view/learning-c-30/9780596155018/ch04s03.html)
* [MS docs - Work with decimal types](https://docs.microsoft.com/en-us/dotnet/csharp/tutorials/intro-to-csharp/numbers-in-csharp-local#work-with-decimal-types)

> The M suffix on the numbers is how you indicate that a constant should use the decimal type. Otherwise, the compiler assumes the double type.

#### .NET Core

* [.NET Interactive](https://github.com/dotnet/interactive)
* [.NET Interactive Plans](https://github.com/dotnet/interactive/issues/392)
* [REPL console application feature request](https://github.com/dotnet/interactive/issues/335)
* [OmniSharp C# REPL feature request](https://github.com/OmniSharp/omnisharp-vscode/issues/1239)
* [dotnet-script](https://github.com/filipw/dotnet-script)

Console application example

```csharp
Console.WriteLine((decimal)5 / (decimal)6);

Console.WriteLine((int)5 / (decimal)6);

Console.WriteLine(5 / 6);

Console.WriteLine(5 / 6.1);

object obj1 = 5;
Console.WriteLine(obj1.GetType());

object obj2 = 6.1;
Console.WriteLine(obj2.GetType());

object obj3 = 6.1m;
Console.WriteLine(obj3.GetType());

int int1 = 5;
Console.WriteLine(int1.ToString());

decimal decimal1 = 6.1m;
Console.WriteLine(int1 / decimal1);
```

Output

```csharp
dotnet run
0.8333333333333333333333333333
0.8333333333333333333333333333
0
0.819672131147541
System.Int32
System.Double
System.Decimal
5
0.819672131147540983606557377
```

#### .NET Framework

* [REPL for full .NET Framework using C# interactive window in VS](https://odetocode.com/blogs/scott/archive/2020/01/21/the-c-interactive-window.aspx)

Interactive window example

```csharp
> 5/6
0
> 5m/6m
0.8333333333333333333333333333
> 5.0/6.0
0.83333333333333337
> 5/6.0
0.83333333333333337
```

#### Mono

* [Mono REPL](https://www.mono-project.com/docs/tools+libraries/tools/repl/)
* [Online REPL repl.it/languages/csharp](https://repl.it/languages/csharp)

Run Mono's REPL using command: `csharp`

```
csharp
Mono C# Shell, type "help;" for help

Enter statements below.
csharp> 
```

### R

* [Online REPL repl.it/languages/rlang](https://repl.it/languages/rlang)

Run R programming language in interactive mode using command: `R`

In interactive mode the last output is in variable: `.Last.Value`

### SQL

```sql
select 6+8 as ans
```

### MATLAB 

In interactive mode the last output is in variable: `ans`

### C#, LINQ and SQL

* [LINQPad](https://www.linqpad.net/CodeSnippetIDE.aspx)
* [RoslynPad](https://github.com/aelij/RoslynPad)

### BC

`bc` or as i call it best calculator.

In interactive mode the last output is in variable: `last`

Can also be used on Termux on Android see my blog post: [Everyday Termux uses](https://2e0pgs.github.io/blog/android/2019/10/11/everyday-termux-uses/)

`dc` useful for polish or reverse polish notation.

### Python

#### Python2.7

```python
>>> 5/6
0
```

#### Python3.6

```python
>>> 5/6
0.8333333333333334
```

## Graphing

Julia programming language.

[GNU Octave](https://www.gnu.org/software/octave/) like MATLAB but open source.

[GNU Octave plot issues](https://stackoverflow.com/a/25733323)

[GNU Plot CSV files](https://raymii.org/s/tutorials/GNUplot_tips_for_nice_looking_charts_from_a_CSV_file.html)

[GNU Plot examples](http://www.malinc.se/math/octave/threeden.php)

RMarkdown

[Jupyter Notebook](https://jupyter.org/)

[Jupyer with .NET Core](https://github.com/dotnet/interactive#notebooks-with-net-core)

Wolfram Mathematica?

## First floor weight

This was going to be a separate blog post but i'll throw it in here. 

Calculating the maximum weight a section of floor supported by joists can handle.

### Quality documentation in simple HTML

* [fet.uwe.ac.uk house ages](https://fet.uwe.ac.uk/conweb/house_ages/elements/section4.htm)
* [bedroom-workshop](http://bedroom-workshop.com/workshop-floorloading/0workshop-floorloading.html#anchortop)

```
anyway 1.5 kN per square meter as per the BS6399-1
F = M*A
so he is correct that assuming A (acceleration) is gravity 9.81 m/s^2
M = F/A
1500/9.81 = 152 kg per square meter
```

I have at least 222 kg within a 1 meter squared area. However the weight is positioned near a load bearing wall.
