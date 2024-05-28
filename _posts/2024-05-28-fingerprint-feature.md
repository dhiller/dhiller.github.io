---
layout: post
title: how to disable and enable fingerprint sensor on Lenovo laptop 
summary: 
tags: [fingerprint,fedora]
---

Since I tend to forget how to disable and re-enable the fingerprint sensor
on my Lenovo Thinkpad, here's how to do it on the shell.

WARNING: this will kill the current Wayland session!

```bash
# disable fingerprint sensor
sudo authselect disable-feature with-fingerprint
```

```bash
# enable fingerprint sensor
sudo authselect enable-feature with-fingerprint
```
