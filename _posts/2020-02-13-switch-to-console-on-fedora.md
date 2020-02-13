---
layout: post
title: Switch to console tty on Fedora 
summary: This article describes how to switch to console tty on Fedora Linux as the "normal" shortcuts don't seem to work 
tags: [linux,fedora,tty,console,gnome]
---

tl;dr Use ALT + CTRL + F3 to switch to console tty3, switch back to recent wm session with ALT + CTRL + F2
---

This is just a quick note on how to switch to command line tty on Fedora Linux. This was tested on Fedora 31.

Today my system seemed frozen, so I wanted to switch to console tty, but none of the shortcuts seemed to work anymore. I used ALT+CTRL+F2 at first, that was what I remembered I had used before on other window systems.

In hindsight I guess it just was a combination of impatience and the system not reacting directly that nearly made me perform a hard switch off. So, to avoid this again, when using Fedora 31 with Gnome and Wayland, use the following:

Shortcut | Action observed
--- | ---
ALT + CTRL + F3 ... ALT + CTRL + F6 | Switch to console TTY3 .. TTY6
ALT + CTRL + F2 | Switch to current window manager session
ALT + CTRL + F1 | Switch to login screen / create new session (?)

**Note: have patience, i.e. wait a couple of seconds, the system might not react directly if a process hammers it.**
