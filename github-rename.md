<!-- github-trials by NewForester:  a series of notes on trials of GitHub and git features -->

# Rename a GitHub Repository

Renaming a GitHub repository may be simple but there are consequences for local working copies.

---

Renaming the GitHub repository at the UI is simple:

 1. Navigate to the repository name page (https://github.com/<user</<repository>).
 1. Select "Settings" tab.
 1. In the "Repository name" (the first) section, type in the new name.
 1. Click on the "Rename" button that is now active.

---

For each local, working copy, the remote repository URL must be changed.

The dumb way is to start over:

    $ git remote -v
    github  git@github.com:NewForester/github-old (fetch)
    github  git@github.com:NewForester/github-old (push)

    $ git remote rm github
    $ git remote add github git@github.com:NewForester/github-new

    $ git remote -v
    github  git@github.com:NewForester/github-new (fetch)
    github  git@github.com:NewForester/github-new (push)

    $ git push -u github

The last command "sets the upstream (tracking) reference" so that `git push` etc. may be used without arguments.
It is not entirely clear any more whether this was necessary or merely done for the sake of completeness.

It may be that:

    $ git remote set-url <name> <newurl> [<oldurl>]
    $ git remote set-url --push <name> <newurl> [<oldurl>]

is the way it should be done but it might still be necessary to run `git push -u`.

---

Copyright (C) 2017, NewForester, not for release or reuse.

<!-- EOF -->
