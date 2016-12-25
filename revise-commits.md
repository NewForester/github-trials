# Commit Revision - Attempt 1

These notes cover the first attempt to reorder/revise a sequence of commits so that it makes more sense.
Something similar has been done before using ```git cherry-pick``` but that was a long time ago and no documentation survives.

Unfortunately, not everything worked as expected and things got confused.
This write-up is more a reconstruction.

On this occasion, a modification to cribtutor was recorded as a sequence of commits:

  - Correct typo in test files
  - Refactor the html parsing by splitting the print policy into annotate()
  - Add test for the markdown list workaround
  - Add the Massage module
  - Integrate Massage module

with a commit of help file changes pending.

The snag was that the last commit actually comprised three changes:
  - Rename annotate() to annotateElement()
  - Integrate Massage module
  - Revise comments

The comment revisions were in the files altered by the Massage module integration, not the (new) Massage module itself.

---

The Atlassian notes suggested that ```git rebase -i``` would do the trick.

I suppose the operation could have been attempted on the 'master' branch
but that would have been asking for trouble.

Far better to do the operation on a branch.
First, checkout the last commit we don't want to change and create a branch from that commit:

    $ git checkout b23a93
    $ git checkout -b cleanup
    $ git rebase -i master

The first two did what was expected and wanted but last one did not.

The idea had been to rebase 'master' on the new branch 'cleanup' and use the interactive feature to rearrange the commits.
This is what the Atlassian notes seemed to suggest you should do.

Perhaps I was missing a flag but I don't see which.

```git rebase -i``` started ```git edit``` session but with no commits to rebase.
On exit from the editor I found 'master' had indeed been rebased on 'cleanup'.

I don't see why this should be so either.

---

Try again.

I seem to remember that:

    $ git rebase -i <commit>

began from the commit after \<commit>.

Also very useful was:

    $ git rebase --abort

when you realised you were starting in the wrong place.

---

I started with a rebase of the last commit as I wanted to split it into three.

    $ git rebase -i <commit>

In the ```git edit``` session I indicated that I wanted to edit the commit.

The Atlassian notes simply say you can split commits this way.
I referred to ```git rebase --help```, the section on "Splitting Commits", for the how.

    $ git reset HEAD^

This puts the working directory into the state before the commit that it being split.

First, I wanted to separate the renaming of annotate() to annotateElement().
This involved only one file:

    $ git add -i Html.cpp

This starts a little session that is at first confusing.
Type 'p' to patch, then '1' to select (the only file) to work through.

The command processing will now present hunks one-by-one.
Type 'y' to stage a hunk, 'n' to leave the hunk unstaged.

At the end you have two copies of the file:  one staged, one unstaged.
You need to commit the staged version.

Second, I decided to stage the comment revisions leaving the integration changes.
I could, and should, have done it the other way around.

    $ git add cribtutor.cpp
    $ git add -i Html.h Html.cpp

All changes to ```cribtutor.cpp``` needed to be staged.
I think I could have done that from within ```git add -i```.
The same probably goes for many times I ran ```git diff``` beforehand.

For 'Html.h' there was a slight problem.
First off, some hunks are run together because there is only one line or so unchanged between them.
Enter 's' for split and then you can decide which hunk splinters to stage, one-by-one.

However, there was a hunk I wanted to split that had no common line.
Enter 'e' for edit.
This started a ```git edit``` session and I edited the hunk by hand.
Conceptually this is not too hard.
The point to remember that you need to remove what you don't want staged.

After committing the comment revisions I wanted to indicate that
what was left of the original commit could now be rebased.

I don't know if this is possible.
I got an error message when I tried ```git rebase --continue```
and I ended up making a third commit because ```git rebase --help``` says ```git rebase --continue``` needs a clean working directory.

---

Now the original 4 commits were 7.  I started another ```git rebase -i```.

I reordered the new commits:
  * the first of the three, the one that renamed annotate(), went after the original patch that added annotate()
  * the second and third were swapped

Then I indicated that the now first two commits should be squashed (combined) into one.

The rebase failed with a merge error.

At the time I was confused.
Perhaps I had staged something I had not meant to.
Perhaps I had got an edit wrong somehow.
Perhaps I had issued the commands in the wrong order.

On reflection, none of the above.

During the second ```git add -i``` I split a hunk by hand in the editor
while producing the second and third commits
and during the third ```git add -i``` I had reordered these two commits.

I suspect there would have been no trouble if I had not (had to) split a hunk by hand
(and do so in a manner that does not match ```git``` default hunking rules).

I'm fairly certain there would have been no trouble
had I not reordered the second and third commits.

So, yes, I should have split them the other way.

---

Next, but not finally, the new set of patches needed applying to the 'master' branch.

This means winding back the head so as to forget the commits that were about to be replaced:

    $ git reset 1f033ea05405c342deadec200704d244e9be0b10

Ideally you do a ```git reset --hard``` to clean the working directory
but I have subdirectories I did not want deleting
so I decided to play safe.

This left me with a dirty working directory I need to clean up by hand:

    $ git checkout Html.cpp Html.h cribtutor.cpp makefile test/cribsheets.txt
    $ rm Massage.cpp Massage.h test/markdown.html test/markdown.inp test/markdown.ref

Now it was possible to merge:

    git merge cleanup

I actually tried this with '-n' in the hope that it would do a dry run but the merge took place anyway.

Consultation of ```git merge --help``` reveals that '-n' is short for '-no-stat'.
There appears to be no dry run option.

Finally, check the program still builds and passes all regression tests.
Clean up temporaries and push changes upstream.

---

The final reordering replaced 4 commits with 6.
The first was the result of a squash - the original commit date was preserved.
The last two were the result of a split - the original commit date was lost.
Shame about the latter.  Perhaps we'll find a way around that too some day.
