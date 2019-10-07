---
layout: post
title: Lowercase all the things
date: 2018-08-05 14:09:13
author: Peter Stevenson
summary: Deciding on a personal file naming convention.
categories: programming
thumbnail:
tags:
 - programming
 - lowercase
 - naming
 - convention
 - standardisation
 - files
---

### Deciding on a personal file naming convention.

If you haven't read my other post on [Programmers grammar](https://2e0pgs.github.io/blog/programming/2018/08/05/programmers-grammar/) I suggest you do so before reading this one for a bit of background.

After much research and consideration I settled on a general naming convention for my files on my filesystem. That convention is lowercase-hyphencase. So words are all lowercase and are separated by a hyphen. Example `my-magic-text-file.txt`. Notice how the only special character is a fullstop designating the beginning of the file type extension.

Numbers may also be present for date stamps or version numbers. Example `2019-05-25-gardening-notes.md`. Notice how the date goes first, this is to aid with sorting/ordering in a directory listing.

#### Reasons

* Easier on autocomplete in terminals. No need to hold shift.
* Is pretty much universally safe for programs to parse.
* It's the web standard for URL format.
* Linux seems to have shifted to this standard from the old `snake_case` format for the core OS filesystem.

This is basically my general naming convention. 

Many subfolders may also contain a `README.md` which hold metadata. This is normally a description of the folder structure if it's unusual.

LF or CRLF?

#### References

* See unix file system spec
* See web url spec
* See jekyll spec
* See JS/Node/Angular spec
