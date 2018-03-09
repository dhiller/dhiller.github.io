---
layout: post
title: Run multiple Intellij version installations on linux
summary: Run multiple Intellij version installations on linux
tags: [linux,intellij,ide,configuration]
---

I love Intellij very much. I like the jetbrains EAP program for trying out new features of the IDE. After being screwed a couple of times replacing a working installation by a faulty one, I elaborated a way of keeping several versions of Intellij in parallel.

As a side note: I'm reporting about a Linux Mint 18 install but I guess most other linux distributions will have minor adjustments to get this approach working.

First of all I like to keep all versions and needed files in a dedicated folder in /opt/IDEA. I have them symlinked so I can more easily switch versions:

    /opt/IDEA ‹›
    ╰─$ ls -la    
    insgesamt 24
    drwxr-xr-x  6 dhiller dhiller 4096 Nov 17 11:17 .
    drwxr-xr-x 13 root    root    4096 Nov 16 15:59 ..
    lrwxrwxrwx  1 dhiller dhiller   13 Nov 14 17:20 current -&gt; idea-2016.2.5
    lrwxrwxrwx  1 dhiller dhiller   11 Nov  9 09:29 eap -&gt; idea-2016.3
    lrwxrwxrwx  1 dhiller dhiller   19 Okt 20 08:18 idea-2016.2.5 -&gt; idea-IU-162.2228.15
    lrwxrwxrwx  1 dhiller dhiller   19 Nov 17 11:09 idea-2016.3 -&gt; idea-IU-163.7743.37
    drwxr-xr-x  9 dhiller dhiller 4096 Okt 20 08:17 idea-IU-162.2228.15
    drwxr-xr-x  9 dhiller dhiller 4096 Nov  9 09:29 idea-IU-163.7342.3
    drwxr-xr-x  9 dhiller dhiller 4096 Nov 14 08:50 idea-IU-163.7743.17
    drwxr-xr-x  9 dhiller dhiller 4096 Nov 17 11:09 idea-IU-163.7743.37
    lrwxrwxrwx  1 root    root      37 Sep  7 11:56 idea.vmoptions -&gt; /home/dhiller/bin/IDEA/idea.vmoptions
    lrwxrwxrwx  1 root    root      43 Sep  7 11:56 run_idea_with_env.sh -&gt; /home/dhiller/bin/IDEA/run_idea_with_env.sh

First of all I have a file where my Intellij options are stored:

File: /opt/IDEA/idea.vmoptions

    -Xms1G
    -Xmx2G
    -XX:MaxPermSize=512m
    -XX:ReservedCodeCacheSize=512m
    -XX:+UseConcMarkSweepGC
    -XX:SoftRefLRUPolicyMSPerMB=50
    -ea
    -Dsun.io.useCanonCaches=false
    -Djava.net.preferIPv4Stack=true
    -XX:+HeapDumpOnOutOfMemoryError
    -XX:-OmitStackTraceInFastThrow
    -Dawt.useSystemAAFontSettings=on
    -Dswing.aatext=true
    -Dsun.java2d.xrender=true

Then I have a shell script:

File: run_idea_with_env.sh

    #!/bin/bash

    if [ ! -d "/opt/IDEA/$1" ]; then
            echo "Usage: $0 "
            exit 1
    fi

    # set environment variables
    export JAVA_HOME="/usr/lib/jvm/default-java/"
    export IDEA_VM_OPTIONS="/opt/IDEA/idea.vmoptions"

    /opt/IDEA/$1/bin/idea.sh

Last but not least I have two or three desktop definitions to be able to start the IDE from the Menu in ~/.local/share/applications:

File: jetbrains-idea.desktop

    #!/usr/bin/env xdg-open
    [Desktop Entry]
    Version=1.0
    Type=Application
    Name=IntelliJ IDEA
    Icon=/opt/IDEA/current/bin/idea.png
    Exec="/opt/IDEA/run_idea_with_env.sh" current
    Comment=The Drive to Develop
    Categories=Development;IDE;
    Terminal=false
    StartupWMClass=jetbrains-idea

File: jetbrains-idea-eap.desktop

    #!/usr/bin/env xdg-open
    [Desktop Entry]
    Version=1.0
    Type=Application
    Name=IntelliJ IDEA EAP
    Icon=/opt/IDEA/eap/bin/idea.png
    Exec="/opt/IDEA/run_idea_with_env.sh" eap
    Comment=The Drive to Develop
    Categories=Development;IDE;EAP
    Terminal=false
    StartupWMClass=jetbrains-idea

The key point is the line starting with Exec that points to the version to start:

    Exec="/opt/IDEA/run_idea_with_env.sh" current

But you just could start the IDE from a terminal with:

    $ /opt/IDEA/run_idea_with_env.sh current

for starting the latest stable or

    $ /opt/IDEA/run_idea_with_env.sh eap

for starting the latest EAP version,

When installing a new version I just download it, untar it into /opt/IDEA, change the symlink to point to the new version, and that's it.
