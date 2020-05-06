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

# Deciding on a personal file naming convention.

If you haven't read my other post on [Programmers grammar](https://2e0pgs.github.io/blog/programming/2018/08/05/programmers-grammar/) I suggest you do so before reading this one for a bit of background.

After much research and consideration I settled on a general naming convention for my files on my filesystem. That convention is lowercase-hyphencase. So words are all lowercase and are separated by a hyphen. Example `my-magic-text-file.txt`. Notice how the only special character is a fullstop designating the beginning of the file type extension. I choose to always append a file extension where possible.

Numbers may also be present for date stamps or version numbers. Example `2019-05-25-gardening-notes.md`. Notice how the date goes first, this is to aid with sorting/ordering in a directory listing.

Many subfolders may also contain a `README.md` which hold metadata. This is normally a description of the folder structure if it's unusual.

Folders also follow this naming convention. Example: `pictures`.

Any common words across several filenames should go before the differentiating words. For example: `myfile-magic-cat.txt`, `myfile-magic-dog.txt`

LF or CRLF? A tricky one, I should probably enforce one type on my NAS. My Git repos are always LF server side.

This is basically my general naming convention. Program source files have their own conventions: [2E0PGS/codingstyle](https://bitbucket.org/2E0PGS/codingstyle)

## Reasons

* Easier on autocomplete in terminals. No need to hold shift.
* Is pretty much universally safe for programs to parse.
* It's the web standard for URL format.
* Linux seems to have shifted to this standard from the old `snake_case` format for the core OS filesystem.

## References

* [Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.pdf)
* [Uniform Resource Locators (URL)](https://tools.ietf.org/html/rfc1738)
* [Jekyll structure](https://jekyllrb.com/docs/structure)
* [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html#file-name)
* [Angular coding style guide](https://angular.io/guide/styleguide#symbols-and-file-names)
