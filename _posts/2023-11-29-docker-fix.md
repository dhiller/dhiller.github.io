---
layout: post
title: containerd broken - how to workaround on Fedora 39
summary: 
tags: [docker,containerd,fedora,f39]
---

`containerd-1.6.23-1` is broken on Fedora 39. This causes running containers with `docker` to fail. I normally use `podman` but for some tasks I rely on docker. I.e. when testing `prow` jobs locally, I use `phaino`, which seems to use containerd under the hood.

The issue itself is tracked in [GitHub CoreOS tracker](https://github.com/coreos/fedora-coreos-tracker/issues/1578) and [Bugzilla](https://bugzilla.redhat.com/show_bug.cgi?id=2237396). From there you quickly get to the workaround, which is downgrading containerd to `containerd-1.6.19-1.fc39`.

Here's how to do it:
* Download [rpm](https://koji.fedoraproject.org/koji/buildinfo?buildID=2165951) for your architecture
* Do `dnf localinstall` of the rpm
* Do a versionlock on the rpm
* remind yourself to remove the version lock after the fix got in

```bash
# install dnf versionlock plugin
dnf install 'dnf-command(versionlock)'
# get the rpm
cd /tmp
wget https://kojipkgs.fedoraproject.org//packages/containerd/1.6.19/1.fc39/x86_64/containerd-1.6.19-1.fc39.x86_64.rpm
# install it
dnf localinstall containerd-1.6.19-1.fc39.x86_64.rpm
# lock the version
dnf versionlock containerd
# restart docker daemon
systemctl restart docker
```
