---
layout: post
title: Jupyter notebooks for C#/SQL
date: 2021-04-03 01:02:19
author: Peter Stevenson
summary: Jupyter notebooks in VS Code and ADS.
categories: programming
thumbnail:
tags:
 - .NET
 - C#
 - SQL
 - VS Code
---

# Jupyter notebooks for C#/SQL

This article is mostly based around ADS (Azure Data Studio) version: 1.25.3

## Technologies involved

* Jupyter notebooks
* .NET Interactive
* SSMS
* Azure Data Studio
* .NET 5
* VS Code
* SQL

## Some usage notes

* You will still need a local or remote SQL server to run the SQL kernel.
* Memory seems more than SSMS, 369 MB Azure Data Studio vs 150 MB SSMS 2017.
* 416 MB Visual Studio 2017 vs VS Code 361 MB.
* Use Azure Data Studio for SQL.
* Use VS Code for .NET.
* Azure Data Studio uses `sql-data-tools` like the mssql VS Code extension.
* Azure Data Studio allows for notebooks and _graphing?_ etc unlike mssql extension for VS Code.
* Much like VS Code the project management is done by opening folders primarily. This is useful to have in Azure Data Studio because I can have a window open with all my notebooks (querying ad-hoc or investigatory response) and a window open with all my `.sql` scripts (report or DBA tasks).
* Could do with an option for markdown as the default for new text cells.
* ADS `ctrl + r` don't work but F5 does, tab key moves across column but right arrow doesn't.
* Doesn't allow for multiple SQL server connections per a notebook yet.
* Cant call a `.sql` script from inside the code cell yet. See: [azuredatastudio/issues/8467](https://github.com/microsoft/azuredatastudio/issues/8467)
* ADS cant select columns/rows via shift key but can individually via `ctrl` or totally via top left corner. See: [azuredatastudio/issues/2727](https://github.com/microsoft/azuredatastudio/issues/2727)
	* Can however drag to highlight results.
* ADS Scrolling can be a bit confusing.
	* It's simply because the mouse falls inside a code block which steals the scroll wheel.
	* Try position pointer to the far left or right hand side to avoid that happening.
* SSMS 2018 installs a copy of Azure Data Studio (system) by default
* SSMS 2018 still doesn't have a dark theme option in settings without modifying a reg key.
* VS Code seemed to try opening the notebook using a different kernel e.g. Pyspark instead of Python 3.
	* This is a notebook created/saved in ADS and opened in VS Code.

## Questions I had

* `.ipynb` extension vs ".NET Interactive" notebooks `.dib` extension?
	* Looks like they favour the prior currently.
* Exporting to PDF?
* APIs? Use JavaScript, C# or Python as a client.
* bc support? (basic calculator) or as I call it (best calculator).
* Bash scripts?
* Why UI isn't the same as VS Code notebooks? Shared UI lib and/or better end user continuity.

## Python oddities between VS Code and ADS

I was doing some testing of Python vs C# regarding the ability to comment out indented code blocks quickly. Specifically without having to shift indentation around. 

During my testing I noticed some disparities between ADS and VS Code Python kernel.

* Why Python behaves differently?
	* Perhaps ADS has some different flags enabled on the Python interpreter?
	* Perhaps ADS uses IPython and VS Code uses regular Python? The prior maybe less strict with indentation?
	* Perhaps Python version differences?
* VS Code seems to use system installs whereas Azure Data Studio has a pre-bundled Python?
* I tried creating fresh notebooks in each instead of creating in one and opening in other. The same behaviour was observed.

### Invoking the ADS bundled Python manually

_Behaves as expected. Python forcing us to indent properly for readability and in some cases functionality due to lack of braces in the language._

```python
C:\Users\peter\azuredatastudio-python\0.0.1\python.exe Desktop\test.py
  File "Desktop\test.py", line 2
    print("test")
    ^
IndentationError: unexpected indent
```

Invoking the system installation of Python manually.

_Behaves as expected._

```python
C:\Windows\py.exe Desktop\test.py
  File "C:\Users\peter\Desktop\test.py", line 2
    print("test")
IndentationError: unexpected indent
```

### ADS Python

_Surprised it didn't moan at me._

![azure-data-studio-python](/blog/assets/2021-04-03/azure-data-studio-python.png)

### VS Code Python

_Behaves as expected._

![vs-code-python](/blog/assets/2021-04-03/vs-code-python.png)

Digging a bit further I tried this in [binder](#other-tools) and noticed something similar.

### Binder classic notebook IPython

_Surprised it didn't moan at me._

![binder-ipython-classic](/blog/assets/2021-04-03/binder-ipython-classic.png)

### Binder new JupyterLab Python 3

_Behaves as expected._

![binder-python3-new](/blog/assets/2021-04-03/binder-python3-new.png)

### ADS .NET 5 C# 

_Behaves as expected. We use braces for scope therefore it's never forcing us to whitespace indent properly which is useful for quickly commenting out a few blocks of surrounding code for debugging purposes. See: [C# if-else statement: Curly braces or not? An in-depth analysis](https://social.technet.microsoft.com/wiki/contents/articles/37763.c-if-else-statement-curly-braces-or-not-an-in-depth-analysis.aspx)_

![azure-data-studio-csharp](/blog/assets/2021-04-03/azure-data-studio-csharp.png)

## Articles

* [How to Use .NET Interactive Jupyter Notebooks in Daily Work-Life \| Data Exposed: MVP Edition](https://www.youtube.com/watch?v=W-F0gO7dVOE)
	* Show preview kernels for current notebook provider.
* [.NET Interactive is here! \| .NET Notebooks Preview 2](https://devblogs.microsoft.com/dotnet/net-interactive-is-here-net-notebooks-preview-2/)
* [Jupyter kernels - List of available kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels)
* [Jupyter notebook with multiple languages](https://z-uo.medium.com/jupyter-with-multiple-languages-1ae58e98dc8e)
* [The November 2019 release of Azure Data Studio is now available](https://cloudblogs.microsoft.com/sqlserver/2019/11/05/the-november-2019-release-of-azure-data-studio-is-now-available/?ocid=AID754288&wt.mc_id=azfr-c9-scottha&wt.mc_id=CFID0530)
	* Not entirely sure why I included this link in my draft. Perhaps it was referral from another doc judging the ref id.
* [Azure Data Studio FAQ](https://docs.microsoft.com/en-us/sql/azure-data-studio/faq?view=sql-server-ver15)
	* Specifically the following FAQ questions are interesting: 
		* "Now that there's Azure Data Studio, does Microsoft plan to deprecate SSMS and SSDT?" 
		* "When Should I Use Azure Data Studio or SQL Server Management Studio?"
* [What is Azure Data Studio?](https://docs.microsoft.com/en-us/sql/azure-data-studio/what-is-azure-data-studio?view=sql-server-ver15)

### Also see

* [2E0PGS blog - A state of .NET in late 2020](https://2e0pgs.github.io/blog/programming/2021/01/10/a-state-of-dotnet-in-late-2020/#interactive)
	* I touch on .NET Interactive Notebooks and include a link to Philip Carter's presentation with F# notebooks.
* [2E0PGS blog - REPL maths and notation](https://2e0pgs.github.io/blog/programming/2020/09/27/repl-maths-and-notation/)
	* Some Jupyter kernels are built on top of REPLs.

## Videos

* [Bringing .NET Interactive to Azure Data Studio Notebooks](https://www.youtube.com/watch?v=938jBJ-tK3c)
* [Jupyter Notebooks in Azure Data studio \| Azure Data Studio Tutorial](https://www.youtube.com/watch?v=I5Qz9jLnAqk)
* [Introduction to Azure Data Studio Notebooks \| Data Exposed](https://www.youtube.com/watch?v=Nt4kIHQ0IOc)
* [How to use Jupyter Notebooks in Azure Data Studio \| Azure Friday](https://youtu.be/pHuRj9ty9cI)
* [Plotting Graphs in C# with XPlot and Jupyter](https://www.youtube.com/watch?v=dUPpm-2KINE)
* [VSCode's Python Interactive mode is AMAZING!](https://www.youtube.com/watch?v=lwN4-W1WR84)

## Other tools

* [nteract.io](https://nteract.io/)
	* Interactive apps using notebooks and REPLs.
* XPlot F# graphing lib.
* [Use Jupyter with .NET Interactive on Binder](https://github.com/dotnet/interactive/blob/main/docs/NotebooksOnBinder.md)
	* [mybinder.org/v2/gh/dotnet/interactive](https://mybinder.org/v2/gh/dotnet/interactive/main?urlpath=lab)
		* No syntax highlighting on the mybinder currently.
* [binder](https://mybinder.org/) "Turn a Git repo into a collection of interactive notebooks"
* [Azure Notebooks](https://notebooks.azure.com/)
* [replit](https://repl.it/repls) online REPL environment.
* [C# Shell (C# Offline Compiler)](https://play.google.com/store/apps/details?id=com.radinc.csharpshell) Android PlayStore app.
* [IPython](https://ipython.org/) Interactive Python
* [Jupyter - Try Jupyter](https://jupyter.org/try)

