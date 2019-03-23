
# git Cheat Sheet

by [Ben Nadel][bennadel] for _future Ben Nadel_

The `git` API is so vast, I can barely manage to keep a fraction of one-percent of it in my head. As such, I wanted to outline a few of the commands that I commonly (and sometimes uncommonly) reach for. This way, when I inevitably get stuck and my brain fails me, I have something that I can refer back to.

Future Ben, you are welcome!

## I want to show the status of the current branch.

```sh
git status
```

## I want to create a new branch based on the current branch.

```sh
git checkout master

# Creates a new branch, `my-feature`, based on `master`.
git checkout -b my-feature
```

## I want to checkout the previous branch that I was on.

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

## I want to list the files that have been modified in the current working tree.

By default, when you call `git diff`, you see all of the content that has been modified in the current working tree (and not yet staged). However, you can use the `--stat` to simply list the files that have been modified:

```sh
git diff --stat
```

## I want to view the changes made in a given commit.

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

## I want to view the changes made across multiple commits.

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

## I want to view the changes made in a given file.

By default, the `show` command shows all of the changes in a given commit. You can limit the scope of the output by using the `--` and identifying a filepath:

```sh
# Outputs the changes made to the `README.md` file in the `head` commit of the
# `my-feature` branch.
git show my-feature -- README.md
```

## I want to view the contents of a file in a given commit.

By default, the `show` command shows you the changes made to a file in a given commit. However, if you want to view the entire contents of a file as defined at in a given commit, regardless of the changes made in that commit, you can use the `:` to identify a filepath:

```sh
# Outputs the contents of the `README.md` file as defined in the `head` commit
# of the `my-feature` branch.
git show my-feature:README.md

# Outputs the contents of the `README.md` file as defined in the `19e771`
# commit.
git show 19e771:README.md
```

## I want to open the contents of a file in a given commit in my editor.

Since you're working on the command-line, the output of any git-command can be piped into another command. As such, you can use the `show` command to open a previous commit's file-content in your editor or viewer of choice:

```sh
# Opens the `README.md` file from the `head` commit of the `my-feature` branch
# in the Sublime Text editor.
git show my-feature:README.md | subl

# Opens the `README.md` file from the `19e771` commit in the `less` viewer.
git show 19e771:README.md | less
```

## I want to copy a file from a given commit into my current working tree.

Normally, the `checkout` command will update the entire working tree to point to the given commit. However, you can use the `--` modifier to copy (or checkout) a single file from the given commit into your working tree:

```sh
git checkout my-feature

# While staying on the `my-feature` branch, copy the `README.md` file from
# the `master` branch into the current working tree. This will overwrite the
# current version of `README.md` in your working tree.
git checkout master -- README.md
```

## I want to copy the last commit from another branch into my branch.

When you don't want to merge a branch into your current working tree, you can use the `cherry-pick` command to copy specific commit-changes into your working tree. Doing so creates a new commit on top of the current branch:

```sh
git checkout master

# Copy the `head` commit-changes of the `my-feature` branch onto the `master`
# branch. This will create a new commit on `master`.
git cherry-pick my-feature
```

## I want to copy an earlier commit on the current branch to the `head`.

Sometimes, after you understand why reverted code was breaking, you want to bring the reverted code back into play and then fix it. You _could_ use the `revert` command in order to "revert the revert"; but, such terminology is unattractive. As such, you can `cherry-pick` the reverted commit to bring it back into `head` where you can then fix it and commit it:

```sh
git checkout master

# Assuming that `head~~~` and `19e771` are the same commit, the following are
# equivalent and will copy the changes in `19e771` to the `head` of the 
# current branch (as a new commit).
git cherry-pick head~~~
git cherry-pick 19e771
```

## I want to update the files in the current commit.

If you want to make changes to a commit after you've already committed the changes in your current working tree, you can use the `--amend` modifier. This will add any staged changes to the existing `head` commit.

```sh
git commit -m "Woot, finally finished!"

# Oops, you forgot a change. Edit the file and stage it.
git touch oops.txt
git add .

# Adds the currently-staged changes (oops.txt) to the current `head` commit,
# giving you a chance to updated the commit message.
git commit --amend
```

## I want to copy `master` into my feature branch.

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

When working on a team, where everyone uses different git workflows, you will definitely _want_ a "merge commit". This way, multi-commit merges can be easily be reverted. To force a "merge commit", you can use the `--no-ff` modifier:

```sh
# Get the `my-feature` branch ready for merge.
git checkout my-feature
git rebase master

# Merge the `my-feature` branch into `master` creating a merge-commit.
git checkout master
git merge --no-ff my-feature
```






Notes:

git revert commit -m 1

git reset --hard origin/master

git pull master --rebase

... "already on branch master"

I want to list the files in a given commit




[bennadel]: https://www.bennadel.com

