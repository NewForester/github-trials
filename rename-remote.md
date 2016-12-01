Change the name of the remote repository

---

There is a convention that the remote repository is named 'origin'.
That seems fair enough if your background is cloning Open Source repositories from an arbitrary Internet sources.

It seems fine is you set up the default so you never need to type 'origin' again.

It also seems fine if you only ever have one remote repository.
There is nothing to stop you having more but, in such circumstances, they can't all be named 'origin'.
Perhaps one will clearly be the "one repository to rule them all".
Perhaps not, in which case you would need a better name.

In the current case, why 'origin' and not 'github' ?
That fits better with the idea that the local and the remote repositories are peer repositories.

So, how to change the name of the remote repository from 'origin' to 'github' without changing the URL ?

This will need _get remote_ but also the defaults set by _git push -u_ will need changing.  Hmm ..

---

Changing the  _git push -u_ parameters looking like being a little tricky.

First up, I could not find again any of the _git push_ descriptions I've read recently somewhere in the on-line documentation.
Par for the course.

So try _git push --help_ and look for the description of the _-u_ flag.
That refers to _git config --help_.  Good.

There are three _git config_ levels:
  * --system - appears to be absent on my system (can't find an empty file)
  * --global - (aka user local), no, not in there but I didn't expect it would be
  * --local - (aka repository local), which is where I would expect it to be

There is no _git config --local --list_ but there is _git config --local --edit_, which in this case is not convenient.

Instead:
```
    $ cat .git/config
    [core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
    [branch "master"]
    [remote "origin"]
        url = https://github.com/NewForester/github-tst
        fetch = +refs/heads/*:refs/remotes/origin/*
```
Only one reference to 'origin', nothing there to connect it with 'master' for _git push_ and the like.

Better not edit the _config_ file but get _git_ to do it for us.

----

Changing the remote repository name is easy ... even easier than adding a new definition and removing the old one (or vice versa):
```
    $ git remote -v
    origin  https://github.com/NewForester/github-tst (fetch)
    origin  https://github.com/NewForester/github-tst (push)

    $ git remote rename origin github

    $ git remote -v
    github  https://github.com/NewForester/github-tst (fetch)
    github  https://github.com/NewForester/github-tst (push)
```
Note that just plain
```
    $ git remote
    github
```
is sufficient.

I found the _git remote rename_ from entering _git remote --help_ as you would expect.
Trouble is with _git_ (I still find), is remembering which command you need in the first place.
This time I knew it would be _git remote_ so finding what I needed was no problem.

---

Now to fix up _git _push_.
On its own, it does not work.

So try the obvious:
```
    $ git push -u github master

    $ cat .git/config
    [core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
    [branch "master"]
    [remote "github"]
        url = https://github.com/NewForester/github-tst
        fetch = +refs/heads/*:refs/remotes/github/*
    [branch "master"]
        remote = github
        merge = refs/heads/master
```
Ah, now you're talking.

There is now an explicit relationship between branch 'master' and remote 'github', which was what I was expecting.

So there is something special about 'origin' after all.

---

Copyright (C) 2016, NewForester, not for release or reuse.
