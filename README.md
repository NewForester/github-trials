# github-tst
A repository for trying out GitHub operations.

---

There is nothing of interest to anyone else in this repository.

It is simply a place to try out things described in GitHub articles, make mistakes and learn from those risk free.

---

First up ... upload a new-to-GitHub repository from one I prepared earlier.

It seems a repository still needs to be created in GitHub.

Create it nice and empty and then push your own.

The instructions are here:

[Adding an existing project to GitHub using the command line] (https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/).

You won't actually be able to see the instructions unless you enable JavaScript for _githum.com_.

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

What ?  RTFM.  I did read _man git push_ and I've read it again now that I know what I was looking for.
I've clearly a long way to go with learning _git_ before that description will make any sense.

All it really tells me (now) is that using this flag means the parameters are assumed for _git pull_ and other, unspecified, commands
in contexts unknown.
Hmm ... I'll have to watch that one.

That's why I make my own notes.
It also a lot easier to make your own notes when you've read what other have to say on the matter and have then actually tried it out for yourself.

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

Copyright (C) 2016, NewForester, not for release or reuse.
