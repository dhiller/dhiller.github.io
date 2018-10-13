---
layout: post
title: Fetch own commits for last X days
summary: Fetch own commits for last X days 
tags: [svn,log]
---

TLDR:
Here's how to fetch the commits you yourself did for the last X days. The example does it for last two weeks:

```bash
svn log -v -r {$(date --date="14 day ago" +%Y-%m-%d)}:HEAD --search <user-name>
```

Explanation:
In addition to just using revision numbers `svn log -r` supports dates and keywords also. Date syntax is pretty easy, just put the date with reverse date format (see [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)) into curly braces for the start and/or end point and there you go.

Mixed notations are supported also btw.

What I wanted to achieve here was not having to enter the start date every single time, so I was using `date` to calculate the start date on the fly.
