
# git Cheat Sheet

by [Ben Nadel][bennadel] for _future Ben Nadel_

The `git` API is so vast, I can barely manage to keep a fraction of one-percent of it in my head. As such, I wanted to outline a few of the commands that I commonly (and sometimes uncommonly) reach for. This way, when I inevitably get stuck and my brain fails me, I have something that I can refer back to.

Future Ben, you are welcome!

## I want to create a new branch based on the current branch.

```sh
git checkout master

# Creates a new branch, `my-feature`, based on `master`.
git checkout -b my-feature
```

## I want to checkout the previous branch that I was on.

In some of the `git` commands, the `-` token refers to the last branch that you had checked-out. This makes it very easy to jump back and forth between two branches:

```sh
git checkout master
git checkout my-feature

# The `-` refers to the `master` branch.
git checkout -

# The `-` refers to the `my-feature` branch.
git checkout -
```

This can also be used to merge-in the last branch you were one.

```sh
git checkout my-feature
git add .
git commit -m "Awesome updates."
git checkout master
git merge - # `-` refers to the `my-feature` branch.
```


## I want to show the status of the current branch.

```sh
git status
```







[bennadel]: https://www.bennadel.com

