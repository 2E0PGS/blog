---
layout: post
title: Run Jekyll GitHub pages locally
date: 2020-11-07 22:36:19
author: Peter Stevenson
summary: Debug a Jekyll GitHub page which hasn't been run locally before
categories: programming
thumbnail:
tags:
 - Jekyll
---

# Debug a Jekyll GitHub page which hasn't been run locally before

I was adding a `posts-by-tags` page to my Jekyll blog. Previously I have gotten away with never running the Jekyll site locally thanks to GitHub actions however while adding a new feature I came across a generic error I needed to debug locally.

## Undefined method gsub for integer

Below is the error message:

```
Liquid Exception: undefined method `gsub' for 2003:Integer
```

Upon searching this someone else had a similar issue but this didn't help me to narrow this down enough to fix it: [Liquid Exception: undefined method `gsub' for 2012:Integer in /_layouts/post.html](https://github.com/jekyll/jekyll/issues/6194)

Looking at the official Jekyll docs we get some pointers on how to setup the local development environment: [https://jekyllrb.com/docs/](https://jekyllrb.com/docs/)

* I run the following commands which clone my blog to the local machine. 
* The Jekyll new will need to have `blog` replaced with whatever your repos name is.
* The `--force` flag will overwrite anything that conflicts with a Jekyll template config. 
* `Gemfile` and `.gitignore` is really all it adds, you should still run with `--force` so the URLs are `localhost` not `github.io`
* I just scrap the changes in `git` from `jekyll new` besides the ones I manually made to patch the bugs once I am done debugging.

```sh
git clone https://github.com/2E0PGS/blog.git

jekyll new blog --force

cd blog

bundle exec jekyll serve
```

My particular bug turned out to be I had a tag which was just a lone int: `- 2003`

I fixed and finished implementing my `posts-by-tags` page in commits: 

* [4ebab2dbcbd9bd49e4b43db686934c5a454824df](https://github.com/2E0PGS/blog/commit/4ebab2dbcbd9bd49e4b43db686934c5a454824df)
* [deceef47d6bd7578aa3f94d644847e28196ba056](https://github.com/2E0PGS/blog/commit/deceef47d6bd7578aa3f94d644847e28196ba056)
* [49019a3b688f74a32130e4d2b0273090580cf6f3](https://github.com/2E0PGS/blog/commit/49019a3b688f74a32130e4d2b0273090580cf6f3)

## RSS feed

After fixing the first bug I then came across a second bug thanks to a large code block I had in another post with lots of special chars.

```
Jekyll Feed: Generating feed for posts
```

I fixed this in commit: [af7a60060b2bab05d0805d1443e1674a4d8e26b9](https://github.com/2E0PGS/blog/commit/af7a60060b2bab05d0805d1443e1674a4d8e26b9) with my custom variable: `feed_exclude_content: true`

## TODO

Checkout the following variables more:

* `thumbnail:`
* `series:`
* `site.related_posts`

Add the actual RSS feed error instead of the line before the error.