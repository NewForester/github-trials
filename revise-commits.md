<!-- github-trials by NewForester:  a series of notes on trials of GitHub and git features -->

# Revising Commits

These notes are on how to change one or more commits to a `git` repository.

As recommended elsewhere, only do this on a local repository before the commits in question have be pushed to remote repositories.

If you have the means of backing up your repositories, do so before you start.
Getting it wrong can permanently loose data you do not want to loose.
Getting it right may take practice.
You have been warned.

It is also a good idea to work with a clean directory so,
until you are comfortable that you know what you are doing,
stash your work before you start.

---

The simplest case of commit revision is the correction of a typographical error in the commit message of the most recent commit on a branch.
Make sure you have the branch concerned checked out.

Check the commit message you want to revise is the most recent one using:

    git log -n 1

Make sure that you have no files in the staging area using:

    git status

You need to unstage them because otherwise they will be committed when you change the commit message.

To change the commit message:

    git commit --amend

This will invoke your configured `git editor` where you can change the commit message to your heart's content.

---

The next simplest case is to alter the files (or their contents) of the most recent commit on a branch.

The procedure is the same but instead of unstaging all changes, make sure the staging area contains the changes you want included in the revised commit.
Then run the `git commit --amend` as before.

---

To alter older commits you exploit the `git rebase -i` command.

Normal `git` work-flow would have you:

  1. create a branch
  1. make changes on the branch
  1. rebase the branch on its parent and immediately
  1. merge the branch with its parent (usually 'master')
  1. delete the branch.

The purpose of the rebase it to bring the branch up to date with respect to its parent.
You can do this as often as you like.
I certainly am not yet aware of circumstances when this would be a bad idea.

`git rebase -i` (with the extra `-i`) also lets you re-order, edit and otherwise alter the commits on the branch.
This is feature that is wanted.

You can bring the branch up to date by rebasing without `-i` option
to deal with any conflicts due the recent changes on the main branch and
then rebase with `-i` over and over until you have thoroughly revised the commits on the branch.

If the branch is short lived, you will be able to revise any or all of the commits on the branch.
If the branch is longer lived and you have previously merged commits on the branch back into the main branch
then you will only be able to revise the commits on the branch made since the most recent merge.

The same goes for 'master', where you can revise only those commits since the last push.
The same, in effect, goes for any branch that has been pushed to any repository.

Revising anything further back is, or is tantamount to, modification of commits that have been published.
Technically possible but potentially very unpopular.

---

The third case of commit revision is when you want to alter the commit message of an older commit on the branch.
<!-- -->

    git rebase -i <parent>

will rebase against the `<parent>` branch, usually, 'master' and invoke
the `git editor` in which are listed the commits on the branch (oldest first).

Each has the word 'pick' beside it.
Change 'pick' to 'reword' for the commit(s) whose message(s) you want to change, save and exit.

The `git editor` will now be invoked so you can change the commit message, just as for `git commit --amend`.
It will be invoked for each commit you flagged with 'reword'.

---

The fourth case is when you want to alter the files (or their contents) of an older commit on the branch.
There are two ways of doing this.

Proceed as before but replace 'pick' with 'edit', save and exit.

The rebase will stop just after replaying, but before recommitting, the commit you want to alter.
You are now free to do as you want but you need to finish off as the `git` suggests with:

    git commit --amend
    git rebase --continue

The working directory needs to be clean for the rebase to continue.

This process may be a little risky so you might want to do some testing before committing.

If you decide you are in the sheep dip, you can cancel the whole rebase with:

    git rebase --abort

and work on mustering up the nerve to try again.

---

The alternative way of altering an older commit is to make and test your changes;
commit them and only then run rebase.

The new commit with your revisions will appear at the bottom of the commit list.

Change 'pick' to 'squash' or 'fixup' and move the commit in the editor
so that it is listed immediately after the commit you want to alter.
Save and exit.

The rebase will combine the two commits, keeping the who and when of the older commit.
It will invoke the `git editor` so that you can amend the commit message.

Use 'squash' and the newer commit's message is commented out.
Use 'fixup' and the older commit's message is commented out.
Either way, in the editor you are free to devise the commit message of your choice.

---

Note so far there has been no mention of using `git reset`.
This command is more about throwing work away than amending it.

This sounds risky but it is handy for moving commits onto a banch.

It is recommended you always work on a (sub)branch and never on the main branch (typically 'master').

Suppose you have been not being doing so and now, oh rat scat, you need to make a urgent change and release it.
What you need to do now is move the more recent commits off 'master' onto a branch.

This allows you to sort out the urgent problem (on a branch of course) that you can rebase, merge and push.
It also sets the current work up the way it should have been done in the first place.

Use `git log` to identify the last commit that should stay on 'master' (say the commit with the id `123abc`).
Create the branch and then populate it with the more recent commits:

    git branch work 123abc
    git checkout work
    git rebase master

<!-- -->
Now you can go back to the master and remove the more recent commits:

    git checkout master
    git reset --hard 123abc

<!-- -->
The more recent commits have now been safely moved from 'master' to 'work' and
you can now get on with the urgent work on its own branch.
Once that has been completed, merged and pushed, you can switch to the 'work' branch,
rebase it on 'master' and carrying on safely.

Here is a simpler way of achieving the same result:

    git branch work
    git reset --hard 123abc
    git checkout work

---

The final example is how to alter the first commit in a repository.

If you create a local repository instead of cloning a remote repository,
it is quite easy to forget to create a branch and work on that
until you are several commit in.

Following the instructions above,
all but the first commit can be moved onto a branch.

All that is left on 'master' is the first commit.
It can now be altered using `git commit --amend` as needed.

Rebase your work branch before continuing.

---

<div>Copyright (C) 2017, NewForester, not for release or reuse.</div>

<!-- EOF -->
