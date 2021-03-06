<!-- github-trials by NewForester:  a series of notes on trials of GitHub and git features -->

# Upload to GitHub a pre-exisiting local git repository

---

First trial ... upload a new-to-GitHub repository from one I prepared earlier.

It seems a repository still needs to be created in GitHub.

Create it nice and empty and then push your own.

The instructions are here:

[Adding an existing project to GitHub using the command line] (https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/).

You won't actually be able to see the instructions unless you enable JavaScript for *github.com*.

---

These instructions end with ...

    git push origin master

... which is perfectly adequate.

I remember reading somewhere that this leaves you needing to specify 'origin' and 'master'
with every push and that using a particular option on the first push means you won't need to specify these parameters again.

I'm sure I read that somewhere in the (introductory) GitHub pages but I'm blowed if I can find it.
This seems to be a perennial problem with documentation you haven't written yourself, particularly with
wiki style documentation such as used on GitHub.

Don't get me wrong, I love wiki documentation but I always end up making my own notes (if only where to look in the wiki).
After all, the help article under which I found the right description wasn't the first one I tried.
I'm still learning that in the GitHub cloud a 'project' is a 'repository' and vice versa.

I did make my own notes.  The command I was looking for is:

    git push -u origin master

What ?  RTFM.  I did read `man git push` and I've read it again now that I know what I was looking for.
I've clearly a long way to go with learning `git` before that description will make any sense.

All it really tells me (now) is that using this flag means the parameters are assumed for `git pull` and other, unspecified, commands
in contexts unknown.
Hmm ... I'll have to watch that one.

That's why I make my own notes.
It also a lot easier to make your own notes when you've read what others have to say on the matter and have then actually try it out for yourself.

Hence this repository.

---

Ah.  First surprise is that creating a very empty repository comes up with a page that gives you notes on the command line options to:
  * do quick setup
  * create a new repository on the command line
  * create an existing repository from the command line
  * import code from a (hosted) Subversion, Mercurial, or TFS project.

Nice.

The worrying bit is the '.git' suffix on the GitHub URL.
This implies a 'raw' repository (in GitHub) that cannot be modified via GitHub.

This does not look like the same destination by a different route.
However, I don't seem to be able to rename the GitHub URL.

---

After pushing just one commit with one file things have settled down.
The GitHub interface has all the buttons there for creating news files and editing existing one.

The GitHub URL no longer has the '.git' suffix.
While it may not be necessary to change the remote repository settings locally, it can be done:

    git remote -v
    origin  https://github.com/NewForester/github-tst.git (fetch)
    origin  https://github.com/NewForester/github-tst.git (push)
    git remote rm origin

    git remote add origin https://github.com/NewForester/github-tst
    github-tst$ git remote -v
    origin  https://github.com/NewForester/github-tst (fetch)
    origin  https://github.com/NewForester/github-tst (push)

---

Conclusion:

You have to create a repository in GitHub anyway so it is simpler to do that, clone the new repository, copy in your files and push.

Only if you already have a `git` repository with history that you want to host on GitHub is the palaver described above worth it.
It is not a lot of work but there is scope for mistakes all the more likely because this is not something you practise everyday.

If you have a project already hosted somewhere else (possibly not even in a `git` repository) then it would appear GitHub has facilities
for importing that may do what you want without the need to use a local copy and the command line.

---

If you follow the GitHub instructions for creating a repository, you create a README file and GitHub gleans the repository description from this.

If as above, you create an empty repository there is no README file from which to glean a description.

If you want to add one, navigate to the "Code" tab of the repository's main page.
Just above the tabbar, you will see a grey note that the repository has no description and no web-page.

To the right of this is an "Edit" button.
Click on this button and enter your description.

You can use the "Edit" button any time to amend the description.

---

Copyright (C) 2016, NewForester, not for release or reuse.

<!-- EOF -->
