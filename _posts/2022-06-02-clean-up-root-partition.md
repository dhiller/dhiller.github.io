---
layout: post
title: How to clean up root partition on a Fedora Linux
summary: How to clean up the root partition on a Fedora Linux installation
tags: [linux,root,cleanup,diskspace]
---

I recently had the disk space problem again, so I resized the partitions as described in an [earlier article]({% post_url 2020-02-13-resize-lvm-encrypted-file-systems-on-fedora %}) to give my home partition a bit more space. I needed this as I have a lot of projects in my working projects directory, additionally I am preparing a talk for a conference, where I need to convert virtual machine file system files, and those are quite big.

You might think that due to current harddrive capacities you wouldn't run into this problem, but when you are using containers as a vehicle for doing stuff, you might need to download those, which can lead to having stored a lot of images that might even be very big. Thus you can run quickly into that situation where the root partition is filled up with 50GB, which in my case happens pretty often.

Tip 1: determine where the space gets used
---

To be the most effective you should know where the space gets eaten up, so start Disk Usage Analyzer to give you a neat overview of where the most space is used.

Tip 2: clean up docker stuff
---

If you still are using docker as a container runtime then you might have a lot of image files that you might not need any more:

```sh
$ docker system prune -a
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all images without at least one container associated to them
  - all build cache

Are you sure you want to continue? [y/N]
...
```

You might also have some dangling volumes around that might get needed to clean up:
```sh
$ docker volume prune
WARNING! This will remove all local volumes not used by at least one container.
Are you sure you want to continue? [y/N]
...
```

Tip 3: vacuum your journald log files
---

As my install is pretty old, since I always do an upgrade instead of a clean reinstall whenever a new Fedora version comes out, I had quite a bit of log files dangling around:

```sh
# journalctl --disk-usage
Archived and active journals take up 3.9G in the file system.
[root@dhiller-fedora-work opt]# journalctl --vacuum-size=1G
...
Vacuuming done, freed 2.9G of archived journals from /var/log/journal/de0c95d62c9a48e59f67e64d59542e3b.
[root@dhiller-fedora-work opt]# journalctl --disk-usage
Archived and active journals take up 1.0G in the file system.
```


Sources:
* https://forums.fedoraforum.org/showthread.php?301185-low-disk-space-on-quot-filesystem-root-quot
* https://www.linuxuprising.com/2019/10/how-to-clean-up-systemd-journal-logs.html
* https://developers.redhat.com/blog/2020/12/10/how-to-clean-up-the-fedora-root-folder
