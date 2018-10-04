---
layout: post
title: Use gpg-agent for svn password caching 
summary: Use gpg-agent for svn password caching
tags: [svn,gpg-agent]
---

How to configure gpg-agent for in memory svn password caching
===

Install gpg agent:

```bash
sudo apt-get install gnupg-agent
```

Make sure pinentry is present:

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

