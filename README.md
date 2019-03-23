
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

```sh
git checkout my-feature
git add .
git commit -m "Readme updates."

git checkout master
# Outputs the changes made in the `head` commit of the current (`master`) branch.
git show
# Outputs the changes made in the `head` commit of the `my-feature` branch.
git show my-feature
```














[bennadel]: https://www.bennadel.com

