---
layout: post
title: Use gpg-agent for svn password caching 
summary: Use gpg-agent for svn password caching
tags: [svn,gpg-agent]
---

Here's how to configure gpg-agent for in memory svn password caching.

Install gpg agent:

```bash
$ sudo apt-get install gnupg-agent
```

Make sure pinentry is present. Usually pinentry-curses is present, to avoid the curses version, install `pinentry-tty` and configure it to be used for gpg in `~/.gnupg/gpg-agent.conf`:

```bash
$ sudo apt-get install pinentry-tty
$ echo "pinentry-program /usr/bin/pinentry-tty" >> ~/.gnupg/gpg-agent.conf
$ gpg-connect-agent reloadagent /bye
```

~/.bash_profile
```
...
GPG_TTY=$(tty)
export GPG_TTY
eval $(gpg-agent --daemon)
```

Configure subversion to use gpg as password storage:

~/.subversion/config
```
[auth]
password-stores = gpg-agent
```

~/.subversion/servers
```
[global]
store-passwords = yes
store-plaintext-passwords = no
```

Sources:
* [SVN Encrypted Password Storage](https://wiki.apache.org/subversion/EncryptedPasswordStorage)
* [pinentry-tty](https://superuser.com/a/521027)
