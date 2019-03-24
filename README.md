
# git Cheat Sheet

by [Ben Nadel](https://www.bennadel.com) for _future Ben Nadel_

The `git` API is so vast, I can barely manage to keep a fraction of one-percent of it in my head. As such, I wanted to outline a few of the commands that I commonly (and sometimes uncommonly) reach for. This way, when I inevitably get stuck and my brain fails me, I have something that I can refer back to.

Future Ben, you are welcome!

## Table of Contents

* [I want to show the status of the current branch.](#i-want-to-show-the-status-of-the-current-branch)
* [I want to create a new branch based on the current branch.](#i-want-to-create-a-new-branch-based-on-the-current-branch)
* [I want to checkout the previous branch that I was on.](#i-want-to-checkout-the-previous-branch-that-i-was-on)
* [I want to list the files that have been modified in the current working tree.](#i-want-to-list-the-files-that-have-been-modified-in-the-current-working-tree)
* [I want to view the changes made in a given commit.](#i-want-to-view-the-changes-made-in-a-given-commit)
* [I want to view the changes made across multiple commits.](#i-want-to-view-the-changes-made-across-multiple-commits)
* [I want to view the changes made in a given file.](#i-want-to-view-the-changes-made-in-a-given-file)
* [I want to view the contents of a file in a given commit.](#i-want-to-view-the-contents-of-a-file-in-a-given-commit)
* [I want to open the contents of a file in a given commit in my editor.](#i-want-to-open-the-contents-of-a-file-in-a-given-commit-in-my-editor)
* [I want to copy a file from a given commit into my current working tree.](#i-want-to-copy-a-file-from-a-given-commit-into-my-current-working-tree)
* [I want to copy the last commit from another branch into my branch.](#i-want-to-copy-the-last-commit-from-another-branch-into-my-branch)
* [I want to copy an earlier commit on the current branch to the `head`.](#i-want-to-copy-an-earlier-commit-on-the-current-branch-to-the-head)
* [I want to update the files in the current commit.](#i-want-to-update-the-files-in-the-current-commit)
* [I want to copy `master` into my feature branch.](#i-want-to-copy-master-into-my-feature-branch)
* [I want to revert the merge of my feature branch into `master`.](#i-want-to-revert-the-merge-of-my-feature-branch-into-master)
* [I want to undo the changes I've made to my branch.](#i-want-to-undo-the-changes-ive-made-to-my-branch)
* [I want to remove unpublished changes from my branch.](#i-want-to-remove-unpublished-changes-from-my-branch)

## Use Cases

### I want to show the status of the current branch.

```sh
git status
```

### I want to create a new branch based on the current branch.

```sh
git checkout master

# Creates a new branch, `my-feature`, based on `master`.
git checkout -b my-feature
```

### I want to checkout the previous branch that I was on.

In some of the `git` commands, the `-` token refers to the "last branch" that you had checked-out. This makes it very easy to jump back-and-forth between two branches:

```sh
git checkout master
git checkout my-feature

# At this point, the `-` refers to the `master` branch.
git checkout -

# At this point, the `-` refers to the `my-feature` branch.
git checkout -
```

The `-` token can also be used to merge-in the last branch that you had checked-out:

```sh
git checkout my-feature
git add .
git commit -m "Awesome updates."
git checkout master

# At this point, the `-` refers to the `my-feature` branch.
git merge -
```

The `-` token can also be used to `cherry-pick` the most recent commit of the last branch that you had checked-out:

```sh
git checkout my-feature
git add .
git commit -m "Minor tweaks."

git checkout master

# At this point, the `-` refers to the `my-feature` branch.
git cherry-pick -
```

### I want to list the files that have been modified in the current working tree.

By default, when you call `git diff`, you see all of the content that has been modified in the current working tree (and not yet staged). However, you can use the `--stat` to simply list the files that have been modified:

```sh
git diff --stat
```

### I want to view the changes made in a given commit.

When `show` is given a branch name, it will default to `head` - the last / most-recent commit on the given branch:

```sh
git checkout master

# Outputs the changes made to the `head` commit of the current (`master`)
# branch.
git show

# Outputs the changes made to the `head` commit of the `my-feature` branch.
git show my-feature
```

You can also use the `show` command to target a specific commit that is not the `head` commit. This can be done with a specific commit hash; or, a relative commit operator like `~`:

```sh
# Outputs the changes made in the commit with the given hash.
git show 19e771

# Outputs the changes made in in a previous commit of the current (`master`)
# branch.
git show head~ # Show changes in first parent.
git show head~~ # Show changes in first parent's first parent.
git show head~~~ # Show changes in first parent's first parent's first parent.

# Outputs the changes made in a previous commit of the `my-feature` branch.
git show my-feature~
git show my-feature~~
git show my-feature~~~
```

### I want to view the changes made across multiple commits.

While the `show` command can show you changes in a given commit, you can use the `diff` command to show changes across multiple commits:

```sh
git checkout master

# Outputs the changes between `head~` and `head` of the current branch. If
# only one commit is provided, other commit is assumed to be `head`.
git diff head~

# Outputs the changes between the first commit and the second commit.
git diff head~~~..head~~
```

And, since branch names are really just aliases for commits, you can use a branch name in order to show the changes between one branch and your branch:

```sh
git checkout my-feature

# At this point, the following are equivalent and output the changes between
# the `head` commit of the `master` branch and the `head` commit of the
# `my-feature` branch.
git diff master
git diff master..head
git diff master..my-feature
```

### I want to view the changes made in a given file.

By default, the `show` command shows all of the changes in a given commit. You can limit the scope of the output by using the `--` and identifying a filepath:

```sh
# Outputs the changes made to the `README.md` file in the `head` commit of the
# `my-feature` branch.
git show my-feature -- README.md
```

### I want to view the contents of a file in a given commit.

By default, the `show` command shows you the changes made to a file in a given commit. However, if you want to view the entire contents of a file as defined at in a given commit, regardless of the changes made in that commit, you can use the `:` to identify a filepath:

```sh
# Outputs the contents of the `README.md` file as defined in the `head` commit
# of the `my-feature` branch.
git show my-feature:README.md

# Outputs the contents of the `README.md` file as defined in the `19e771`
# commit.
git show 19e771:README.md
```

### I want to open the contents of a file in a given commit in my editor.

Since you're working on the command-line, the output of any git-command can be piped into another command. As such, you can use the `show` command to open a previous commit's file-content in your editor or viewer of choice:

```sh
# Opens the `README.md` file from the `head` commit of the `my-feature` branch
# in the Sublime Text editor.
git show my-feature:README.md | subl

# Opens the `README.md` file from the `19e771` commit in the `less` viewer.
git show 19e771:README.md | less
```

### I want to copy a file from a given commit into my current working tree.

Normally, the `checkout` command will update the entire working tree to point to the given commit. However, you can use the `--` modifier to copy (or checkout) a single file from the given commit into your working tree:

```sh
git checkout my-feature

# While staying on the `my-feature` branch, copy the `README.md` file from
# the `master` branch into the current working tree. This will overwrite the
# current version of `README.md` in your working tree.
git checkout master -- README.md
```

### I want to copy the last commit from another branch into my branch.

When you don't want to merge a branch into your current working tree, you can use the `cherry-pick` command to copy specific commit-changes into your working tree. Doing so creates a new commit on top of the current branch:

```sh
git checkout master

# Copy the `head` commit-changes of the `my-feature` branch onto the `master`
# branch. This will create a new commit on `master`.
git cherry-pick my-feature
```

### I want to copy an earlier commit on the current branch to the `head`.

Sometimes, after you understand why reverted code was breaking, you want to bring the reverted code back into play and then fix it. You _could_ use the `revert` command in order to "revert the revert"; but, such terminology is unattractive. As such, you can `cherry-pick` the reverted commit to bring it back into `head` where you can then fix it and commit it:

```sh
git checkout master

# Assuming that `head~~~` and `19e771` are the same commit, the following are
# equivalent and will copy the changes in `19e771` to the `head` of the 
# current branch (as a new commit).
git cherry-pick head~~~
git cherry-pick 19e771
```

### I want to update the files in the current commit.

If you want to make changes to a commit after you've already committed the changes in your current working tree, you can use the `--amend` modifier. This will add any staged changes to the existing `head` commit.

```sh
git commit -m "Woot, finally finished!"

# Oops, you forgot a change. Edit the file and stage it.
touch oops.txt
git add .

# Adds the currently-staged changes (oops.txt) to the current `head` commit,
# giving you a chance to updated the commit message.
git commit --amend
```

### I want to copy `master` into my feature branch.

At first, you may be tempted to simply `merge` your `master` branch into your feature branch, but doing so will create a Frankensteinian commit tree. Instead, you should `rebase` your feature branch on `master`. This will ensure that your feature commits are cleanly colocated in the commit tree:

```sh
git checkout my-feature

# This will unwind the commits specific to the `my-feature` branch, pull in
# the `master` commits, and then replay your `my-feature` commits.
git rebase master
```

Once your `my-feature` branch has been rebased on `master`, you could then - if you wanted to - perform a `--ff-only` merge of your feature branch back into `master`:

```sh
git checkout master

# Fast-forward merge of `my-feature` changes into `master`, which means there
# is no creation of a "merge commit."
git merge --ff-only my-feature
```

That said, when you;re working on a team where everyone uses different git workflows, you will definitely _want_ a "merge commit". This way, multi-commit merges can be easily reverted. To force a "merge commit", you can use the `--no-ff` modifier:

```sh
# Get the `my-feature` branch ready for merge.
git checkout my-feature
git rebase master

# Merge the `my-feature` branch into `master` creating a merge-commit.
git checkout master
git merge --no-ff my-feature
```

Now, if the merge needs to be reverted, you can simply revert the "merge commit" and all commits associated with the merge will be reverted.

### I want to revert the merge of my feature branch into `master`.

If you performed a `--ff-only` merge of your feature branch into `master`, there's no "easy" solution. You either have to reset the branch to an earlier commit (rewriting history); or, you have to revert the individual commits in the merge.

If, however, you performed a `--no-ff` merge that created a "merge commit", all you have to do is revert the merge commit:

```sh
git checkout master

# Merge the feature branch in, creating a "merge commit".
git merge --no-ff my-feature

# On noes! You didn't mean to merge that in. Assuming that the "merge commit"
# is now the `head` of `master`, you can revert back to the commit's fist
# parent, the `master` branch: -m 1.
git revert -m 1 head
```

### I want to extract changes I accidentally made to `master`.

Sometimes, after you've finished working on your feature branch, you execute `git checkout master`, only find that you've been accidentally working on `master` the whole time (error: "Already on 'master'"). To fix this, you can `checkout` a new branch and `reset` your `master` branch:

```sh
git checkout master

# Once on the `master` branch, create the `my-feature` branch as a copy of the
# `master` branch. This way, your `my-feature` branch will contain all of your
# recent changes.
git checkout -b my-feature

# Now that your changes are safely extracted, get back into your `master` 
# branch and `reset` it with the `--hard` modifier so that your local index
# and file system will match the remote copy.
git checkout master
git reset --hard origin/master
```

### I want to undo the changes I've made to my branch.

If you've edited some files and then change your mind about keeping those edits, you can reset the branch using the `--hard` modifier. This will update the working tree - your file structure - to match the structure of the last commit on the branch (`head`).

**Caution**: You will lose data when using the `--hard` option.

```sh
git checkout my-feature
touch temp.txt
git add .

# Remove the file from staging AND remove the changes from the file system.
git reset --hard
```

If you call `git reset` without the `--hard` option, it will reset the staging to match the `head` of the branch, but it will leave your file system in place. As such, you will be left with "unstaged changes" that can be modified and re-committed.

### I want to remove unpublished changes from my branch.

If you've committed changes to the local copy of a remote (ie, published) branch, but you want to undo those changes, you can `reset` the local branch to match the remote branch:

```sh
git checkout my-feature

# Update the remote copy of the `my-feature` branch in order to make sure that
# you are working with the most up-to-date remote content.
git fetch origin my-feature

# Now, reset the local copy of `my-feature` to match the published copy. This
# will update your index and your local file system to match the published
# version of `my-feature`.
git reset --hard origin/my-feature
```

### I want to see which branches have already been merged into `master`.

From any branch, you can locate the merged-in branches (that can be safely deleted) by using the `--merged` modifier:

```sh
git checkout master

# List all of the local branches that have been merged into `master`. This
# command will be relative the branch you currently have checked-out.
git branch --merged
```

### I want to see which branches have not yet been merged into `master`.

From any branch, you can locate the unmerged branches by using the `--no-merged` modifier:

```sh
git checkout master

# List all of the local branches that have NOT YET been merged into `master`.
# This command will be relative the branch you currently have checked-out.
git branch --no-merged
```

### I want to delete my feature branch.

After you're merged your feature branch into `master`, you can delete your feature branch using the `branch` command:

```sh
# Merge your `my-feature` branch into `master` creating a "merge commit."
git checkout master
git merge --no-ff my-feature

# Safely delete the merged-in `my-feature` branch. The `-d` modifier will
# error-out if given branch has not yet been merged into the current branch.
git branch -d my-feature
```

If you want to abandon a feature branch, you can use the `-D` modifier to force delete it even if it has not yet been merged into `master`:

```sh
git checkout master

# Force-delete the `my-feature` branch even though it has not been merged into
# the `master` branch.
git branch -D my-feature
```

### I want to delete a remote branch.

When you delete a branch using `git branch -d`, it deletes your local copy; but, it doesn't delete the remote copy from your origin (GitHub). To delete the remote copy, you have to `push` the branch using the `:` prefix:

```sh
git checkout master

# Safely delete your local copy of the `my-feature` branch. The `-d` modifier
# will error-out if the given branch has not been fully-merged into `master`.
git branch -d my-feature

# Delete the remote copy of the `my-feature` branch from the origin. The `:`
# prefix sends this through as a "delete" command for the given branch.
git push origin :my-feature
```

### I want to update `master` because my `push` was rejected.

If you've committed changes to `master` but you forgot to `pull` recent changes from the remote `master` branch, your next `push` will be rejected with an error that looks like, _"Updates were rejected because the tip of your current branch is behind its remote counterpart"_. To fix this, you can use the `--rebase` modifier:

```sh
git checkout master
git merge --no-ff- my-feature

# Oh noes! You forgot to pull in the latest remote copy of `master` before you
# merged your `my-feature` commits. No problem, just `--rebase` your local
# `master` on the remote branch. This will move your local changes to the tip
# of the `master` branch.
git pull --rebase 

# Now that you've pulled-in the remote changes, you should be able to push
# your updated `master` branch.
git push origin master
```






Notes:

git pull master --rebase

I want to list the files in a given commit

Remove a staged file from the index.
