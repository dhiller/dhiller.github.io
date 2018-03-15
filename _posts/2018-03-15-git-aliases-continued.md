---
layout: post
title: Git aliases continued
summary: Git aliases, continued
tags: [git,alias]
---

Here I'm just jotting down some handy `git aliases` I've become accustomed to use.

Note: shown example output is for the [spring-integration-samples](https://github.com/spring-projects/spring-integration-samples) repository.

__Git status abbreviated__

```bash
git config --global alias.s=status
```

Example output:

    $ git s
    On branch master
    Your branch is up-to-date with 'origin/master'.

    nothing to commit, working tree clean

__Git log enhanced with bells and whistles__

Shows list of commits, using abbreviated commit ids, full comment, and branch graph decorated with tag and branch names

```bash
git config --global alias.ll=log --abbrev-commit --graph --decorate
```

Example output:

    $ git ll
    ...
    *   commit 078a72e
    |\  Merge: 9759373 e53d49d
    | | Author: Gunnar Hillert <gunnar@hillert.com>
    | | Date:   Sun Mar 10 13:02:33 2013 -0700
    | |
    | |     Merge pull request #89 from ghillert/INTSAMPLES-107
    | |
    | |     INTSAMPLES-107 - Add CoffeeService Sample to Oracle StoredProc Sample
    | |
    | * commit e53d49d
    |/  Author: Gunnar Hillert <ghillert@vmware.com>
    |   Date:   Fri Mar 8 16:25:56 2013 -0500
    |
    |       INTSAMPLES-107 - Add CoffeeService Sample to Oracle StoredProc Sample
    |
    |       * Add the CoffeeService Sample from the PostgreSql Stored Procedure Sample to the Oracle Stored Procedure Sample.
    |
    |       For reference see: https://jira.springsource.org/browse/INTSAMPLES-107
    |
    *   commit 9759373 (tag: v2.2.0.RELEASE)
    |\  Merge: 7928871 f6e4f4d
    | | Author: Gunnar Hillert <gunnar@hillert.com>
    | | Date:   Thu Jan 10 22:48:51 2013 -0800
    | |
    | |     Merge pull request #84 from ghillert/INTSAMPLES-102-2
    | |
    | |     INTSAMPLES-102 - Fix intermittently failing tests
    | |
    | * commit f6e4f4d
    |/  Author: Gunnar Hillert <ghillert@vmware.com>
    ...

__Git log enhanced with bells and whistles (short form)__

Shows list of commits in one line format, using abbreviated commit ids, first line of comment, and branch graph decorated with tag and branch names

```bash
git config --global alias.l=log --format=oneline --abbrev-commit --graph --decorate
```

Example output:

    $ git l
    ...
    *   078a72e Merge pull request #89 from ghillert/INTSAMPLES-107
    |\
    | * e53d49d INTSAMPLES-107 - Add CoffeeService Sample to Oracle StoredProc Sample
    |/
    *   9759373 (tag: v2.2.0.RELEASE) Merge pull request #84 from ghillert/INTSAMPLES-102-2
    |\
    | * f6e4f4d INTSAMPLES-102 - Fix intermittently failing tests Reference: https://jira.springsource.org/browse/INTSAMPLES-102
    |/
    *   7928871 Merge pull request #78 from ghillert/INTSAMPLES-63
    ...


__Show changes staged for commit__

```bash
git config --global alias.staged=diff --cached
```

Example output:

    $ git add _posts/
    $ git staged
    diff --git a/_posts/2018-03-15-git-aliases-continued.md b/_posts/2018-03-15-git-aliases-continued.md
    new file mode 100644
    index 0000000..a1cc188
    --- /dev/null
    +++ b/_posts/2018-03-15-git-aliases-continued.md
    @@ -0,0 +1,98 @@
    +---
    ...

Hopefully you'll find them useful also, let me know what you think.

Source: https://git-scm.com/book/tr/v2/Git-Basics-Git-Aliases
