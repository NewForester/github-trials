<!-- github-trials by NewForester:  a series of notes on trials of GitHub and git features -->

# A Repository Clone as a Backup

Each repository clone backups all the others.
Using a GitHub repository means your working copy is backed up every time you push.

However, you may have repositories whose contents you do not want to publish or do not want to publish yet on a platform such as GitHub.
It may there this work is on a branch other that 'master' that is not pushed to GitHub automatically that you still want backed up.

---

First task is to create an area somewhere on another machine (venus) where you can backup repositories.
Something like:

    $ ssh nemo@venus
    $ cd /media/nemo/projects
    $ mkdir repos
    $ cd repos

Assuming `git` has already been installed on this machine, the next step is to make a bare clone of the repository to back up:

    git clone --bare nemo@mars:/home/nemo/repos/projectX

This clones the original (local) repository and sets the original to be the 'origin' of the backup:

    $ cd projectX.git/
    /media/nemo/projects/repos/projectX.git

    $ git remote -v
    origin  nemo@mars:/home/nemo/repos/projectX (fetch)
    origin  nemo@mars:/home/nemo/repos/projectX (push)

---

Next, back on mars, the original repository needs to know where the back up is so that 'backing up' is just a case of 'pushing' changes.

    $ git remote add backup nemo@venus:/media/nemo/projects/repos/projectX.git

    $ git remote -v
    backup  nemo@venus:/media/nemo/projects/repos/projectX.git (fetch)
    backup  nemo@venus:/media/nemo/projects/repos/projectX.git (push)
    github  git@github.com:NewForester/projectX (fetch)
    github  git@github.com:NewForester/projectX (push)

So now:

    $ git push backup

will back up the (default) 'master' branch.

The backup clone is quite independent of the clone published on GitHub.

The notes above use `ssh` as a transport.
This requires, naturally, that `ssh` is set up so that mars can `ssh` into venus.
There are other transports but these too may require setup.

---

You might have branches in the original repository that you do not want published on GitHub.
After all, you do not get contribution credit for them.

    $ git branch private
    $ git checkout private

    $ git add liboaddon
    $ git commit -m "Add customisation for personal use only"

    $ git checkout master
    $ git push backup private

Now the backup clone has a copy of the 'private' branch.
However, the branch needs backing up explicitly and, well, one can easily forget.

According to the man page for `git push`:

    $ git push backup :

should push matching branches, so should backup 'master' and 'private'.
However, for this to be satisfactory, you need to push each (local) branch once explicitly.

    $ git push backup --all

pushes all 'refs' under '/refs/heads' so that would push all branches in the original repository
regardless of whether they have been pushed to the backup repository before or not.

    $ git push backup --mirror

pushes all 'refs' under '/refs', so that includes branchs, tags, the stash and so on.
In fact, it backs up (mirrors) everything, which is what you want.

You can make `--mirror` the default by setting the configuration option `remote.<remote>.mirror`:

    $ git config --local remote.backup.mirror true

So now

    $ git push backup

will update the mirror backup repository and:

    $ git push

will update the 'master' branch on GitHub as it did before.

---

Copyright (C) 2017, NewForester, not for release or reuse.

<!-- EOF -->
