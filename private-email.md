# Change GitHub E-mail Address

---

Each _git commit_ records who made the commit, including their given e-mail address.

Publishing my e-mail address on line has in the past resulted in spam.
I abandoned the sites where my e-mail address was in plain view years ago
and have not used those e-mail addresses since but spammers are still.

This has put me off contributing to mailing lists and being active elsewhere on the Internet.
My policy now is never to leave my e-mail address in a public place.

<!-- See https://help.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile/ -->

It is possible to see the e-mail address associated with any commit on GitHub.
All you need to do is select the commit and add _.patch_ to the URL:

    https://github.com/NewForester/cribtutor/commit/2e6f9064eb232a604039296be5de62ff680c133b.patch

I want to change this.

---

<!-- See https://help.github.com/articles/keeping-your-email-address-private/ -->
<!-- See also https://help.github.com/articles/setting-your-email-in-git/ -->

It is possible to use your anonymous e-mail address instead:

    <username>@users.noreply.github.com

You need to tell GitHub about this so that:
  * commits via the GitHub UI have the correct e-mail address
  * contributions are correctly attributed to you.

How to do this ?
  * Log in to your GitHub account
  * Enter "https://github.com/settings/emails" in the address bar
    * You can of course navigate here with a few click by different routes ...
      * ... not all of which worked last time I tried
  * check the "Keep my email address private" box

It seems I had already done this and, yes, _git log_ already showed commits
with my private e-mail address that I remember doing on-line.

Note: It seems you can tell GitHub about more e-mail addresses.
Perhaps to count contributions from both home and work.

---

You also need to change your local _git_ configuration, either for repositories one-by-one:

    $ git config user.email "<username>@users.noreply.github.com"

or for them all at once:

    $ git config --global user.email "<username>@users.noreply.github.com"

I had a repository where:

    $ git log

reported that all but the last commit had the private e-mail address.
This last commit had not been pushed to GitHub so I could simply rewind the history,
change the configuration and commit again:

    $ git reset HEAD~
    Unstaged changes after reset:
    M       README.md

    $ git config user.email "<username>@users.noreply.github.com"
    git config user.email
    <username>@users.noreply.github.com

    $ git add README.md
    $ git commit -m "Corrected README.md"

---

<!-- See https://help.github.com/articles/changing-author-info/ -->

Changing the e-mail address in the _git_ configuration will affect new commits
but has no retroactive affect on existing commits.

It is possible to change the e-mail address for existing commits but this involves re-writing the repository history.
This should be avoided.

If you are going to do it at all, do it as soon as possible.
Every clone and fork and clone of fork unto the n<sup>th</sup> generation will need to be updated too.

The process is simple but not the kind of thing you are likely to work out for yourself unless you are some kind of _git_ guru.

First make a fresh (but temporary) bare clone of your repository:

    $ git clone --bare https://github.com/user/repo.git temp.git
    $ cd temp.git

Next you need to copy, edit and run a shell script that uses the command _git filter-branch_.
I had never heard of this command before ... an indication that it is not for everyday use.

For the script, see [Changing author info](https://help.github.com/articles/changing-author-info/).

With the script you can change both author's name and e-mail based on e-mail address alone.
I removed the lines pertinent to author as I was not trying to hide my identity, only my e-mail address.

The script ran in no time (but the repository was very small).
The correction then needed to be pushed to GitHub:

    $ git push --force --tags origin 'refs/heads/*'

The temporary clone could now be deleted and the real clone updated:

    $ cd <real clone>
    $ git fetch
    $ git rebase

Job done.

Don't try _git pull_:  that will not give you what you want.

The _git fetch_, _git rebase_ pair needs to be run in every clone of every fork of the repository.
It is less of a deal than starting again with new clones and forks.

---

My first trial was with a clone that was up to date.
What if the clone is not up to date ?

    $ git rebase
    Cannot rebase: You have unstaged changes.
    Additionally, your index contains uncommitted changes.
    Please commit or stash them.

Ah. So:

    $ git stash
    $ git rebase
    $ git stash pop

The _git rebase_ re-applied the local commits on top of the changed history.

The _git stash pop_ re-applied the local changes that had not been committed
but the changes that had been staged before stashing were unstaged after the popping.
Beware.

Now it happens that one of the local changes that was reapplied had been committed with the wrong e-mail address.

So try rewriting the history again ...

    $ ../git-author-rewrite.sh
    Cannot rewrite branches: You have unstaged changes.

So it should have been done before popping the stash.

---

One of the joys of learning something new is that you follow the instructions
knowing you are starting 'simple' and will decide later that 'simple' is not good enough.

Wanting to change the e-mail address of old commits is an example of this.

Part of why changing the e-mail address was a big-little deal was
because I did not want to just follow the instructions suspecting
that later I would find side affects I did not like.

So I thought about what those side affects might be and how I would check for them afterwards.
I was pleased that in this instance I found no unsatisfactory side affects:
_git_ and GitHub both seem to think the way I do and did as I would do.

First, the commits attributed to me through my 'real' e-mail address
are still attributed to me through my 'private' GitHub e-mail address.

Second, the shell script does not change the commit timestamps.
So my contribution history was unchanged.

Third, the author's name was not changed.
My name, as it appears in my _git_ configuration, is not my GitHub user id.
So, if I ever need to, I can still tell which commits were made locally and which through the GitHub UI.
