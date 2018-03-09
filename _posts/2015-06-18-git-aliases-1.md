---
layout: post
title: Useful git aliases when working with Maven and Jira
summary: Useful git aliases when working with Maven and Jira
tags: [git,alias]
---

Put these into your `.gitconfig` below the `alias` section:

```bash
    # Return all jira issue keys from commit messages
    issues = !sh -c 'git log --oneline $@ | egrep -o [A-Z]+-[0-9]+ | sort -V | uniq' -
    # Fetch id of latest commit from maven release plugin
    release-commit = !sh -c 'git log --all --grep=maven-release-plugin.*prepare.release --format=%H| head -1'
    # Return all jira issue keys since last usage of maven release plugin
    issues-since-release = !sh -c 'git issues $(git release-commit)..'
```
