---
layout: post
title: Atom style hidden characters Vim
date: 2018-01-28 21:54:19
author: Peter Stevenson
summary: Use the same display icons as Atom IDE
categories: sysadmin
thumbnail:
tags:
 - Linux
---

# Atom style hidden characters in Vim

_Moved here from [original gist](https://gist.github.com/2E0PGS/5b1f2e0b1a618a9e984745bc2779e312)_

To show hidden characters in Vim and use the same display icons as Atom IDE.

Place the following in your `~/.vimrc`

```
" Show hidden characters and use Atom style.
set list
set listchars=tab:»\ ,eol:¬,space:.
```

Update: 2021-02-02: Include example for Atom and VS Code.

VS Code `~/.config/Code/User/settings.json`

```json
"editor.renderWhitespace": "all",
```

Atom `~/.atom/config.cson`

```cson
  editor:
    showInvisibles: true
```