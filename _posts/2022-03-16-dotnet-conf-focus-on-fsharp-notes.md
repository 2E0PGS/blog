---
layout: post
title: .NET Conf focus on F# notes
date: 2022-03-16 20:14:19
author: Peter Stevenson
summary: Notes I took from watching .NET Conf focus on F#
categories: programming
thumbnail:
tags:
 - C#
 - ASP.NET
 - .NET
---

# .NET Conf: Focus on F# notes

<iframe width="1257" height="707" src="https://www.youtube.com/embed/i3qEhwcG7ps" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Yes this has been sat in my drafts for far too long. I suspect since 2021-07-29

* JavaScript backend
* Supports .NET libs
* Supports LINQ
* Has pipes, is this similar to what we have in bash and unix?
* Prefers type inference. 
	* Total opposite of C# which prefers explicit type definition to cut down on run time errors.
	* [How to use python type hinting?](https://www.youtube.com/watch?v=yScuF1UgGU0)
	* [Carl Meyer - Type-checked Python in the real world - PyCon 2018](https://www.youtube.com/watch?v=pMgmKJyWKn8)
	* **Type**Script
* Looks like a scripting language.
* F# interactive in VS Code.
* `let` keyword much like TypeScript.
* I wonder if it will replace TypeScript in some applications.
* [F# for Fun and Profit](https://fsharpforfunandprofit.com/)
	* [Why use F#?](https://fsharpforfunandprofit.com/why-use-fsharp/)
		* \> F# is not cluttered up with coding “noise” such as curly brackets, semicolons and so on.
			* https://fsharpforfunandprofit.com/posts/fvsc-sum-of-squares/ 
			* Reminds me of what I was saying here: [A state of .NET in late 2020 - C# 9](https://2e0pgs.github.io/blog/programming/2021/01/10/a-state-of-dotnet-in-late-2020/#c-9)
			* Perhaps C# 9 is actually getting lots of ideas from F# however I think there needs to be a clear line between them otherwise you muddy the two languages.
* [SAFE Stack](https://safe-stack.github.io/)
	* ASP.NET + F#
	* Fable compiles F# to JavaScript.
	* React for client.
	* Sharing custom types between server and client.
	* Sharing validation between server and client.
* Alt + enter for dotnet fsi repl run current selection
* Dotnet fsi... where is C# fsi? [Integrate csi to dotnet cli #17666](https://github.com/dotnet/roslyn/issues/17666)
* Haskell vs f#...
