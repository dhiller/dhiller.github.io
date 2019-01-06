---
layout: post
title: Set up multiple JDKs with jenv and AdoptOpenJDK
summary: This article describes how to set up multiple JDKs with jenv and AdoptOpenJDK on an OS X workstation
tags: [java,jenv,openjdk,osx,homebrew]
---

Happy new near everyone!

This short article describes how to set up multiple JDKs on a Mac for quick switching. The motivation behind this was that after the upgrade from JDK 10 to
JDK 11 the @MockBeans annotation (originating from spring-boot-test) was not working anymore as methods from `sun.misc.Unsafe` had been [deprecated](https://dzone.com/articles/jdk-11-and-proxies-in-a-world-past-sunmiscunsafe). Due to an unwary `brew` update I suddenly had a new JDK on my system which broke the build of one of my projects.

I should note that this assumes you are using [Homebrew](https://brew.sh/index) as package manager to ease installation of binaries. This not only works for command line tools but also for OS X application, so you should really check it out.

Step 1: Install OpenJDKs from AdoptOpenJDK with brew
===

OpenJDK releases are available in a tap, meaning a separate repository of brew casks. `brew tap` adds the required repository so that you can use the casks in them.

```bash
$ brew tap AdoptOpenJDK/openjdk
```

We can see the available casks like so

```bash
$ brew search /adoptopenjdk/
==> Casks
adoptopenjdk                         adoptopenjdk11                     adoptopenjdk11-openj9                adoptopenjdk9
adoptopenjdk10                       adoptopenjdk11-jre                 adoptopenjdk8
```

Now we can install the OpenJDK versions we want to use:

```bash
$ brew cask install adoptopenjdk11
$ brew cask install adoptopenjdk10
$ brew cask install adoptopenjdk8
```

Step 2: Install and configure jenv
===

Before we start: what is `jenv`?

[`jenv`](http://www.jenv.be/) is a version manager for java installations for the command line. Note that it does not install the JDKs, it just manages the currently active java version and what's more important, per working directory.

```bash
$ brew install jenv
```

I'm using [`zsh`](http://www.zsh.org/) as shell for the command line. If you're interested, have a look at [`oh-my-zsh`](https://ohmyz.sh/) also, this greatly improves the `zsh` experience with loads of plugins for all kinds of tools. To enable `jenv` on `zsh` I execute

```bash
$ echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.zshrc
$ echo 'eval "$(jenv init -)"' >> ~/.zshrc
```

Don't forget to either source your `*rc` or to restart the shell so that the changes get effective.

Step 3: register JDKs with `jenv`
===

`brew` installs the JDKs into the folder `/Library/Java/JavaVirtualMachines/`, we now need to register each of them with `jenv`:

```bash
$ jenv add /Library/Java/JavaVirtualMachines/adoptopenjdk-11.0.2.jdk/Contents/Home/
$ jenv add /Library/Java/JavaVirtualMachines/adoptopenjdk-10.jdk/Contents/Home/
$ jenv add /Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/
```

*NOTE: the pathes may differ on your machine, so please make sure they exist!*

You can check whether the JDKs have been added correctly if you use the `jenv versions` command

```bash
$ jenv versions
* system (set by /Users/dhiller/.jenv/version)
  1.8
  1.8.0.202
  10.0
  10.0.2
  11.0
  11.0.2
  openjdk64-1.8.0.202
  openjdk64-10.0.2
  openjdk64-11.0.2
```

Step 4: Use `jenv` to specify the current JDK to use
===

Now that `jenv` knows the versions you can either set the JDK globally

```bash
$ jenv global 10.0
$ java -version
openjdk version "10.0.2" 2018-07-17
OpenJDK Runtime Environment AdoptOpenJDK (build 10.0.2+13)
OpenJDK 64-Bit Server VM AdoptOpenJDK (build 10.0.2+13, mixed mode)
```

or for a specific project in the project directory:

```bash
$ cd ~/projects/my_project
$ jenv local 1.8
$ java -version
openjdk version "1.8.0_202"
OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_202-b08)
OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.202-b08, mixed mode)

```

To unset the version

```bash
$ jenv global system
```

or

```bash
$ cd ~/projects/my_project
$ jenv local --unset
```

Step 5: [Optional] Remove previous default Oracle JDK install
===

```bash
$ brew uninstall java
```

NOTE: I included this step because I had some interference on the command line, where `jenv` would not be able to override the chosen JDK, meaning that `java -version` still yielded the Oracle JDK. After I uninstalled the default Oracle JDK this went away. If you are using `~.zshrc` or `~/.bashrc` do not forget to remove manually setting `JAVA_HOME` and the like also as `jenv` will do that for you.

Related Articles
===

Article | Source | Coverage
--- | --- | ---
[How do I install Java on Mac OSX allowing version switching?](https://stackoverflow.com/questions/52524112/how-do-i-install-java-on-mac-osx-allowing-version-switching) | Stackoverflow | Contains coverage of other version installers (SDKMAN, Jabba, manual install), manual version switching and the like
