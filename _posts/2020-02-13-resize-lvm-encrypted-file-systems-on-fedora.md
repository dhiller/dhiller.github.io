---
layout: post
title: Resize encrypted file systems for lvm 
summary: This article describes how to resize root and home partition to reliably resize file system without destroying data 
tags: [linux,fedora,filesystem,lvm,encryption]
---

How to reduce Fedora home partition and increase root in an encrypted luks container
===

CAVEAT: I am not responsible for you losing your data trying this! Please backup your data before doing this! If anything goes wrong, data will be lost!!!
---

I've been in the situation where either root or home partition have been low on free disk space, while the other partition had lots of free space available. Therefore the idea is to first shrink the space on the partition where space is available and then give that space to the other partition.
With that said, I've done this a couple of times already and the filesystems were healthy afterwards.

Initial situation:
* Encrypted LUKS container
* root and home are separate partitions within this LUKS container
* either partition is low on disk space

What you need:
* a bootable Fedora Live installation image, see [here](https://docs.fedoraproject.org/en-US/quick-docs/creating-and-using-a-live-installation-image/) how to create this.

**Note: In the bash commands that follow I resize the home partition to 40GB, you should adjust the disk space to something reasonable that fits your need.**

How to resize partitions:

1. **Boot Fedora Live CD/USB**
2. **open LUKS container**
  * open application `Disks`
  * Select luks partition
  * Select lock symbol
  * Enter password for partition
  * now partitions inside container should be visible in GUI
3. **Open terminal window - get root**
  * `sudo su -`
4. **Important: after each of the following steps you should run e2fsck and correct any errors you get**
  * either `e2fsck -f /dev/mapper/fedora_localhost--live-home`
  * or `e2fsck -f /dev/mapper/fedora_localhost--live-root`
5. **shrink home by first resizing the filesystem and then resizing the partition**
  * `resize2fs /dev/mapper/fedora_localhost--live-home 40G`
  * `lvreduce -L 40G /dev/mapper/fedora_localhost--live-home`
6. **resize the root partition to take the free space**
  * `lvextend -l +100%FREE /dev/mapper/fedora_localhost--live-root`
  * `resize2fs /dev/mapper/fedora_localhost--live-root`

See also:
---
* [Create Fedora live installation medium](https://docs.fedoraproject.org/en-US/quick-docs/creating-and-using-a-live-installation-image/)
* [Resize LVM partitions](https://pingtool.org/online-resize-lvm-partitions-shrink-home-extend-root/)
* [LUKS partitions and Cryptsetup](https://unix.stackexchange.com/questions/41091/how-can-i-shrink-a-luks-partition-what-does-cryptsetup-resize-do)
* [Linux Harddisk encryption with LUKS](https://www.cyberciti.biz/hardware/howto-linux-hard-disk-encryption-with-luks-cryptsetup-command/)
* [Gnome Disk Resize](https://help.gnome.org/users/gnome-help/stable/disk-resize.html.en)
* [Stackexchange: Increase root partition by reducing home](https://unix.stackexchange.com/questions/213245/increase-root-partition-by-reducing-home)
* [Stackexchange: Resize luks volumes](https://unix.stackexchange.com/questions/505241/resize-luks-volumes)
* (answer not really helpful though) [Stackexchange: how can I resize encrypted root...](https://unix.stackexchange.com/questions/132807/how-can-i-resize-my-encrypted-root-and-home-partitions-to-give-root-more-space?noredirect=1)
* [Medium: Resize an Encrypted Partition without Breaking your Linux System](https://link.medium.com/LYpW1ozQGW)

