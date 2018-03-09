---
layout: post
title: Git aliases, the easy way
---

After wrestling with the global `.gitconfig` file for installing aliases like a shortcut for status using `vi`, I thought there must be an easier way, and there is (as always when using git):

Create an alias for a compressed log with decorations and graph:

```bash
git config --global alias.l 'log --format=oneline --graph --decorate'
```

So instead of typing

```bash
git log --format=oneline --graph --decorate
```

then you can just enter

```bash
git l
```

and you will get a onelined history output complete with ascii visual graph and annotations for where every branch pointer is ATM :)

    > git l
    * 87e02884a6d45e0d003ffcdccf41098716ca876e (HEAD -> master) Moved files to folder
    * 9d1c1dee120e5a189f562c2fc5d12709ae35102d (origin/master) Latest version
    * 7b34163291fda0b82bf4777d7b9c24a22df9338e Corrected special characters
    * 838ccffe24c7356f52063fc0b6e6c638dcccbd5a Removed lock file
    * 74fff37b8ee77113b0702baeb0f9b85f17e5f7e9 Added gitignore
    * 4b87667f117412034e977f61de8839b251b5ff81 Added conversion script
    * 291eed8a5f44520e8ca706bc6fc7286978fc821c Initial commit

Source: https://git-scm.com/book/tr/v2/Git-Basics-Git-Aliases
