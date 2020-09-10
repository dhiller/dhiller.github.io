---
layout: post
title: Run multiple Intellij version installations on linux - part 2
summary: Run multiple Intellij version installations on linux - part 2
tags: [linux,intellij,ide,configuration]
---

In my [previous article]({% post_url 2016-11-18-run-multiple-intellij-versions %}) I have shared how to run multiple versions of IntelliJ IDEA. Recently I had some funny stuff going on with the desktop items for applications. How to go about adding and updating these I'd like to share in this article, so my future self can have a look how to do that should the need arise.

So, let's start...

I'm working with [Fedora Linux 32](https://getfedora.org/) at the moment, which I can highly recommend for Lenovo laptops (currently using a ThinkPad T480s). Situation at the beginning was like this:

```bash
$ ls -la /opt/ | grep idea
lrwxrwxrwx.  1 root root   19 Aug 17 12:31 idea -> idea-IU-201.8743.12
drwxr-xr-x.  9 root root 4096 Aug 17 12:30 idea-IU-201.8743.12
drwxr-xr-x.  9 root root 4096 Sep  8 12:57 idea-IU-202.6948.69
lrwxrwxrwx.  1 root root   19 Aug 26 12:11 idea-latest -> idea-IU-202.6948.69
```

So I had just added another symlink to be able to switch between stable and latest versions (which is required as I'm using Go plugin which is not always working well with the latest IDEA version).

When copying the desktop item that was present in `/usr/share/applications` ([source](https://developer.gnome.org/integration-guide/stable/desktop-files.html.en)) and changing it a little I had some funny things going on. It just was not showing up.

```bash
 ls -la /usr/share/applications | grep jetbrains     ↵ 0|1 ⎈ kubevirt-prow-jobs/shift-ovirt-org:8443/dhiller@redhat.com/kubevirt-prow-jobs
-rw-------.  1 root    root     262 Sep 10 09:25 jetbrains-idea-latest.desktop
-rw-------.  1 dhiller dhiller  248 Sep 10 09:25 jetbrains-idea-stable.desktop
```

At first I just thought I had forgotten to update Gnome. This is done by pressing `ALT + F2` and entering either `restart` or `r` ([source](https://unix.stackexchange.com/questions/12118/how-do-i-refresh-gnome-3-applications)). This didn't help. I had a look at several settings in the file, only the `IntelliJ stable` item kept showing up.

Stupid me. Look again at the owner of the first entry from above. Yes, it's `root`. Changing it to myself did the trick, I restarted Gnome again and it appeared directly in the application menu. While being at that, it didn't feel right to leave it in `/usr/share/applications`, so I moved them into `~/.local/share/applications` which is supported since a while also.

When thinking about all this, I still felt this was wrong somehow. I have installed the files into `/opt` but the desktop entries are in my local applications directory? This felt awkward somehow again.

And the Gnome developer desktop page clearly states this:
> Place this file in the /usr/share/applications directory so that it is accessible by everyone, or in ~/.local/share/applications if you only wish to make it accessible to a single user.
([source](https://developer.gnome.org/integration-guide/stable/desktop-files.html.en))

Yeah, of course the right solution would be to change owner to root, but make the files readable by everyone and put them into `/usr/share/applications` again.

```bash
 ls -la /usr/share/applications | grep jetbrains     ↵ 0|1 ⎈ kubevirt-prow-jobs/shift-ovirt-org:8443/dhiller@redhat.com/kubevirt-prow-jobs
-rw-r--r--.  1 root    root     262 Sep 10 09:25 jetbrains-idea-latest.desktop
-rw-r--r--.  1 root    root     248 Sep 10 09:25 jetbrains-idea-stable.desktop
```
