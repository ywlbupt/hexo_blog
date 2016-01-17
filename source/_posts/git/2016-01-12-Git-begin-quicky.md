title: git快速开始
date: 2016-01-12 00:49:16
tags: git
description: Create a new reposiitory on th command line
---

## Create a new reposiitory on th command line


1. create a new repository on the command line  
```
    echo "# git_pn" >> README.md
    git init
    git add README.md
    git commit -m "first commit"
    git remote add origin https://github.com/ywlbupt/git_pn.git
    git push -u origin master
```

2. push an existing repository from the command line
```
    git remote add origin https://github.com/ywlbupt/git_pn.git
    git push -u origin master
```

<!-- more -->


## `-u`参数的定义
The key is "argument-less git-pull". When you do a git pull from a branch, without specifying a source remote or branch, git looks at the branch.<name>.merge setting to know where to pull from. git push -u sets this information for the branch you're pushing.

To see the difference, let's use a new empty branch:

    $ git checkout -b test

First, we push without -u:

    $ git push origin test
    $ git pull
    You asked me to pull without telling me which branch you
    want to merge with, and 'branch.test.merge' in
    your configuration file does not tell me, either. Please
    specify which branch you want to use on the command line and
    try again (e.g. 'git pull <repository> <refspec>').
    See git-pull(1) for details.

If you often merge with the same branch, you may want to
use something like the following in your configuration file:

    [branch "test"]
    remote = <nickname>
    merge = <remote-ref>

    [remote "<nickname>"]
    url = <url>
    fetch = <refspec>

See git-config(1) for details.
Now if we add -u:

    $ git push -u origin test
    Branch test set up to track remote branch test from origin.
    Everything up-to-date
    $ git pull
    Already up-to-date.


Note that tracking information has been set up so that git pull works as expected without specifying the remote or branch.


## Reference Link

[What exactly does the "u" do? "git push -u origin master" vs "git push origin master"](http://stackoverflow.com/questions/5697750/what-exactly-does-the-u-do-git-push-u-origin-master-vs-git-push-origin-ma)
