---
layout: post
title: Permalinks
date: 2018-03-08
categories: [web-dev]
---

_This post is inspired by [Matt Gemmell's post](https://mattgemmell.com/permalinks/)._

- Even though most sites are hosted on Linux machines that are case-sensitive a static site should bake correctly on a machine that isn't. Thus as a bare minimum URLs should be case-insensitive and in my opinion case-preserving as well.
- I agree that URLs should minimize noise and thus remove dates in most cases
- Pretty URLs are fine but I don't like them for simple html content since navigating a downloaded version of the site leads to a lot of /index.html files. Thus having a '.html' suffix is better than the /index.html noise. It is Natural URL Structure.
- Computers serve humans so that in the case where it is simple URLs after the host should be mixed case for improved readability.
- One exception is for paths that could become part of the host. For instance an organization name in GitHub is used for the subdomain for its GitHub pages URL.
- My blog posts are under /posts/ because jekyll