---
layout: post
title: Update all git repos via cmd line in a subdir
---

This one is just for me so I don't forget this "one-liner". Updates all git repositories in direct subdirectories. Works on OS X :)


```bash
cwd=$(pwd); for dir in $(find . -type d -not -name '\.*' -maxdepth 1 -print); \
    (current_dir="$cwd/${dir##./}"; echo "Updating $current_dir"; \
    cd $current_dir; git pull --all;)
```
