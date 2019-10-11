
# Git Dicas

Por [Ben Nadel](https://www.bennadel.com) o futuro Ben Nadel.

A [API do `git` é tão vasta](https://git-scm.com/docs) e tão abstrata, que dificilmente poderá se manter uma fração de tudo em mente. Com isso, este é um esboço de alguns poucos comandos que normalmente são usados. Desta maneira, sempre que inevitavelmente a memória falhar, a uma fonte para se referenciar.

Futuro Ben, de nada!

## Table of Contents

* [Eu quero mostrar o status do branch atual.](#Eu-quero-mostrar-o-status-do-branch-atual)
* [Eu quero criar um novo branch baseado no branch atual.](#Eu-quero-criar-um-novo-branch-baseado-no-branch-atual)
* [I want to checkout the previous branch that I was on.](#i-want-to-checkout-the-previous-branch-that-i-was-on)
* [I want to list the files that have been modified in the current working tree.](#i-want-to-list-the-files-that-have-been-modified-in-the-current-working-tree)
* [I want to view the changes that were made in a given commit.](#i-want-to-view-the-changes-that-were-made-in-a-given-commit)
* [I want to list the files that were changed in a given commit.](#i-want-to-list-the-files-that-were-changed-in-a-given-commit)
* [I want to view the changes that were made across multiple commits.](#i-want-to-view-the-changes-that-were-made-across-multiple-commits)
* [I want to view the changes that were made in a given file.](#i-want-to-view-the-changes-that-were-made-in-a-given-file)
* [I want to view the contents of a file in a given commit.](#i-want-to-view-the-contents-of-a-file-in-a-given-commit)
* [I want to open the contents of a file in a given commit in my editor.](#i-want-to-open-the-contents-of-a-file-in-a-given-commit-in-my-editor)
* [Eu quero copiar um arquivo de um determinado commit para meu diretório de trabalho atual.](#eu-quero-copiar-um-arquivo-de-um-determinado-commit-para-meu-diretorio-de-trabalho-atual)
* [I want to copy the last commit from another branch into my branch.](#i-want-to-copy-the-last-commit-from-another-branch-into-my-branch)
* [I want to copy an earlier commit from the current branch to the `head`.](#i-want-to-copy-an-earlier-commit-from-the-current-branch-to-the-head)
* [I want to update the files in the current commit.](#i-want-to-update-the-files-in-the-current-commit)
* [I want to edit the current commit message.](#i-want-to-edit-the-current-commit-message)
* [I want to copy `master` into my feature branch.](#i-want-to-copy-master-into-my-feature-branch)
* [I want to revert the merge of my feature branch into `master`.](#i-want-to-revert-the-merge-of-my-feature-branch-into-master)
* [I want to extract changes that I accidentally made to `master`.](#i-want-to-extract-changes-that-i-accidentally-made-to-master)
* [I want to undo the changes that I've made to my branch.](#i-want-to-undo-the-changes-that-ive-made-to-my-branch)
* [Eu quero remover modificações não puplicadas no meu branch.](#Eu-quero-remover-modificações-não-puplicadas-no-meu-branch)
* [I want to see which branches have already been merged into `master`.](#i-want-to-see-which-branches-have-already-been-merged-into-master)
* [I want to see which branches have not yet been merged into `master`.](#i-want-to-see-which-branches-have-not-yet-been-merged-into-master)
* [I want to delete my feature branch.](#i-want-to-delete-my-feature-branch)
* [I want to delete a remote branch.](#i-want-to-delete-a-remote-branch)
* [I want to update `master` because my `push` was rejected.](#i-want-to-update-master-because-my-push-was-rejected)
* [I want to remove a file from my staging area.](#i-want-to-remove-a-file-from-my-staging-area)
* [I want to squash several commits into one (or more) commits.](#i-want-to-squash-several-commits-into-one-or-more-commits)
* [I want to squash several commits into one commit without using `rebase`.](#i-want-to-squash-several-commits-into-one-commit-without-using-rebase)
* [I want to temporarily set-aside my feature work.](#i-want-to-temporarily-set-aside-my-feature-work)
* [I want to keep my changes during conflict resolution.](#i-want-to-keep-my-changes-during-conflict-resolution)

## Use Cases

### Eu quero mostrar o status do branch atual.

O comando `status` mostra as diferenças entre a arvore de trabalho (Pastas, arquivos e subpastas), o index e o commit `head` (último commit do branch).

```sh
git status
```

### Eu quero criar um novo branch baseado no branch atual.

Em geral, quando você quer implementar novos recursos em um “branch para recursos” de curta duração onde as modificações podem ser isoladas. Você pode usar o comando `checkout` para criar um novo branch baseado no seu branch atual:

```sh
git checkout master

# Criar um novo branch, `my-feature`, baseado no `master`.
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
# ... changes to the working tree (your file system).
git add .
git commit -m "Awesome updates."
git checkout master

# At this point, the `-` refers to the `my-feature` branch.
git merge -
```

The `-` token can also be used to `cherry-pick` the most recent commit of the last branch that you had checked-out:

```sh
git checkout my-feature
# ... changes to the working tree (your file system).
git add .
git commit -m "Minor tweaks."

git checkout master

# At this point, the `-` refers to the `my-feature` branch.
git cherry-pick -
```

### I want to list the files that have been modified in the current working tree.

By default, when you call `git diff`, you see all of the content that has been modified in the current working tree (and not yet staged). However, you can use the `--stat` modifier to simply list the files that have been modified:

```sh
git diff --stat
```

### I want to view the changes that were made in a given commit.

When `show` is given a branch name, it will default to `head` - the last or most-recent commit on the given branch:

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

### I want to list the files that were changed in a given commit.

Just as with `git diff`, you can limit the output of the `git show` command using the `--stat` modifier. This will list the files that were changed in the given commit:

```sh
# Outputs the list of files changed in the commit with the given hash.
git show 19e771 --stat
```

### I want to view the changes that were made across multiple commits.

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

### I want to view the changes that were made in a given file.

By default, the `show` command shows all of the changes in a given commit. You can limit the scope of the output by using the `--` modifier and identifying a filepath:

```sh
# Outputs the changes made to the `README.md` file in the `head` commit of the
# `my-feature` branch.
git show my-feature -- README.md

# Outputs the changes made to the `README.md` file in the `19e771` commit.
git show 19e771 -- README.md
```

### I want to view the contents of a file in a given commit.

By default, the `show` command shows you the changes made to a file in a given commit. However, if you want to view the entire contents of a file as defined at that time of a given commit, regardless of the changes made in that particular commit, you can use the `:` modifier to identify a filepath:

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

### Eu quero copiar um arquivo de um determinado commit para meu diretorio de trabalho atual.

Normalmente, o comando `checkout` vai atualizar todo seu diretório para o ponto desse determinado commit. Contudo, você pode usar o modificador `--` para copiar (ou fazer checkout) de um único arquivo do commit para seu diretório atual:

```sh
git checkout my-feature

# Em quanto você estiver no galho (branch) `my-feature`, copie o arquivo `README.md`
# do galho (branch) `master`, para o seu diretório de trabalho atual. Isto vai
# sobreescrever a versão atual do `README.md` do seu diretório de trabalho.

git checkout master -- README.md
```

### I want to copy the last commit from another branch into my branch.

When you don't want to merge a branch into your current working tree, you can use the `cherry-pick` command to copy specific commit-changes into your working tree. Doing so creates a new commit on top of the current branch:

```sh
git checkout master

# Copy the `head` commit-changes of the `my-feature` branch onto the `master`
# branch. This will create a new `head` commit on `master`.
git cherry-pick my-feature
```

### I want to copy an earlier commit from the current branch to the `head`.

Sometimes, after you understand why reverted code was breaking, you want to bring the reverted code back into play and then fix it. You _could_ use the `revert` command in order to "revert the revert"; but, such terminology is unattractive. As such, you can `cherry-pick` the reverted commit to bring it back into the `head` where you can then fix it and commit it:

```sh
git checkout master

# Assuming that `head~~~` and `19e771` are the same commit, the following are
# equivalent and will copy the changes in `19e771` to the `head` of the 
# current branch (as a new commit).
git cherry-pick head~~~
git cherry-pick 19e771
```

### I want to update the files in the current commit.

If you want to make changes to a commit after you've already committed the changes in your current working tree, you can use the `--amend` modifier. This will add any staged changes to the existing commit.

```sh
git commit -m "Woot, finally finished!"

# Oops, you forgot a change. Edit the file and stage it.
# ... changes to the working tree (your file system).
git add oops.txt

# Adds the currently-staged changes (oops.txt) to the current commit, giving
# you a chance to update the commit message.
git commit --amend
```

### I want to edit the current commit message.

In addition to adding files to the current commit, the `--amend` modifier can also be used to change the current commit message:

```sh
git add .
git commit -m "This is greet."

# Oh noes! You misspelled "great". You can edit the current commit message:
git commit --amend -m "This is great."
```

Note that if you omit the `-m message` portion of this command, you will be able to edit the commit message in your configured editor.

### I want to copy `master` into my feature branch.

At first, you may be tempted to simply `merge` your `master` branch into your feature branch, but doing so will create an unattactive, non-intuitive, Frankensteinian commit tree. Instead, you should `rebase` your feature branch on `master`. This will ensure that your feature commits are cleanly colocated in the commit tree and align more closely with a human mental model:

```sh
git checkout my-feature

# This will unwind the commits specific to the `my-feature` branch, pull in
# the missing `master` commits, and then replay your `my-feature` commits.
git rebase master
```

Once your `my-feature` branch has been rebased on `master`, you could then, if you wanted to, perform a `--ff-only` merge ("fast forward only") of your feature branch back into `master`:

```sh
git checkout my-feature
git rebase master

# Fast-forward merge of `my-feature` changes into `master`, which means there
# is no creation of a "merge commit" - your `my-features` changes are simply
# added to the top of `master`.
git checkout master
git merge --ff-only my-feature
```

That said, when you're working on a team where everyone uses a different git workflow, you will definitely _want_ a "merge commit". This way, multi-commit merges can be easily reverted. To force a "merge commit", you can use the `--no-ff` modifier ("no fast forward"):

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

If, however, you performed a `--no-ff` merge ("no fast forward") that created a "merge commit", all you have to do is revert the merge commit:

```sh
git checkout master

# Merge the feature branch in, creating a "merge commit".
git merge --no-ff my-feature

# On noes! You didn't mean to merge that in. Assuming that the "merge commit"
# is now the `head` of `master`, you can revert back to the commit's fist
# parent, the `master` branch: -m 1. And, since `head` and `master` are the
# same commit, the following are equivalent:
git revert -m 1 head
git revert -m 1 master
```

### I want to extract changes that I accidentally made to `master`.

Sometimes, after you've finished working on your feature branch, you execute `git checkout master`, only find that you've been accidentally working on `master` the whole time (error: "Already on 'master'"). To fix this, you can `checkout` a new branch and `reset` your `master` branch:

```sh
git checkout master
# > error: Already on 'master'

# While on the `master` branch, create the `my-feature` branch as a copy of
# the `master` branch. This way, your `my-feature` branch will contain all of # your recent changes.
git checkout -b my-feature

# Now that your changes are safely isolated, get back into your `master`
# branch and `reset` it with the `--hard` modifier so that your local index
# and file system will match the remote copy.
git checkout master
git reset --hard origin/master
```

### I want to undo the changes that I've made to my branch.

If you've edited some files and then change your mind about keeping those edits, you can reset the branch using the `--hard` modifier. This will update the working tree - your file structure - to match the structure of the last commit on the branch (`head`).

**Caution**: You will lose data when using the `--hard` option.

```sh
git checkout my-feature
# ... changes to the working tree (your file system).
git add .

# Remove the file from staging AND remove the changes from the file system.
git reset --hard
```

If you call `git reset` without the `--hard` option, it will reset the staging to match the `head` of the branch, but it will leave your file system in place. As such, you will be left with "unstaged changes" that can be modified and re-committed.

### Eu quero remover modificações não puplicadas no meu branch.

Se você fez um commit local de um branch remoto (publicado), mas quer reverter essas alterações, você pode `redefinir` a copia local para corresponder com o branch remoto: 

```sh
git checkout my-feature

# Atualize a copia remota do branch `my-feature` para garantir que você está
# trabalhando com a versão mais atualizada do conteúdo remoto.
git fetch origin my-feature

# Agora, redefina a cópia local de `my-feature` para corresponder com a cópia publicada.
# Isso vai atualizar seu index e seu sistema de arquivo local para corresponder com a versão publicada 
# do `my-feature`.
git reset --hard origin/my-feature
```

### I want to see which branches have already been merged into `master`.

From any branch, you can locate the merged-in branches (that can be safely deleted) by using the `--merged` modifier:

```sh
git checkout master

# List all of the local branches that have been merged into `master`. This
# command will be relative to the branch that you have checked-out.
git branch --merged
```

### I want to see which branches have not yet been merged into `master`.

From any branch, you can locate the unmerged branches by using the `--no-merged` modifier:

```sh
git checkout master

# List all of the local branches that have NOT YET been merged into `master`.
# This command will be relative the branch you have checked-out.
git branch --no-merged
```

### I want to delete my feature branch.

After you're merged your feature branch into `master`, you can delete your feature branch using the `branch` command:

```sh
# Merge your `my-feature` branch into `master` creating a "merge commit."
git checkout master
git merge --no-ff my-feature

# Safely delete the merged-in `my-feature` branch. The `-d` modifier will
# error-out if the given branch has not yet been merged into the current
# branch.
git branch -d my-feature
```

If you want to abandon a feature branch, you can use the `-D` modifier to force-delete it even if it has not yet been merged into `master`:

```sh
git checkout master

# Force-delete the `my-feature` branch even though it has not been merged
# into the `master` branch.
git branch -D my-feature
```

### I want to delete a remote branch.

When you delete a branch using `git branch -d`, it deletes your local copy; but, it doesn't delete the remote copy from your origin (ex, GitHub). To delete the remote copy, you have to `push` the branch using the `:` prefix:

```sh
git checkout master

# Safely delete the local copy of your `my-feature` branch. The `-d` modifier
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
git merge --no-ff my-feature

# Oh noes! You forgot to pull in the latest remote copy of `master` before you
# merged your `my-feature` commits. No problem, just `--rebase` your local
# `master` on the remote branch. This will move your local changes to the tip
# of the `master` branch.
git pull --rebase 

# Now that you've pulled-in the remote changes, you should be able to push
# your updated `master` branch.
git push origin master
```

### I want to remove a file from my staging area.

If you accidentally added too many files to the staging area (in preparation for a `git commit`), you can use the `rm --cached` command to remove them from the staging area but keep them in the working tree:

```sh
git add .

# Oh noes! You didn't mean to add all of the files to the staging area. You
# can remove some of the staged files using the `--cached` modifier:
git rm --cached secrets.config
```

If you accidentally added an entire directory to the staging area, you can add the `-r` modifier to recursively apply the `rm` command:

```sh
git add .

# Oh noes! You didn't mean to add the ./config directory. You can recursively
# remove it with the `-r` modifier:
git rm --cached -r config/.
```

When you `rm` files using `--cached`, they will remain in your working tree and will become "unstaged" changes.

### I want to squash several commits into one (or more) commits.

Your commit history is a representation or your personality. It is a manifestation of your self-respect and the respect you have for your team. As such, you will often need to rewrite your feature branch's history before merging it into `master`. This allows you to get rid of intermediary commit messages like, _"still working on it."_ and _"Meh, missed a bug."_. To do this, you can perform an "interactive rebase".

The "interactive rebase" gives you an opportunity to indicate how intermediary commits should be rearranged. Some commits can be "squashed" (combined) together. Others can omitted (remove). And others can be edited. When performing an interactive rebase, you have to tell `git` which commit to use as the starting point. If you're on an up-to-date feature branch, the starting point _should be_ `master`.

```sh
# Create the `my-feature` branch based on `master`.
git checkout master
git checkout -b my-feature

# ... changes to the working tree (your file system).
git add .
git commit -m "Getting close."

# ... changes to the working tree (your file system).
git add .
git commit -m "Missed a bug."

# ... changes to the working tree (your file system).
git add .
git commit -m "Uggggg! Why is this so hard?"

# ... changes to the working tree (your file system).
git add .
git commit -m "Woot, finally got this working."

# At this point, your commit history is sloppy and would bring much shame on
# your family if it ended-up in `master`. As such, you need to squash the
# commits down into a single commit using an interactive rebase. Here, you're
# telling `git` to use the `master` commit as the starting point:
git rebase -i master
```

As this point, `git` will open up an editor that outlines the various commits and asks you how you want to rearrange them. It should look something like this, with the commits listed in ascending order (oldest first):

```sh
pick 27fb3d2 Getting close.
pick e8214df Missed a bug.
pick ce5ed14 Uggggg! Why is this so hard?
pick f7ee6ab Woot, finally got this working.

# Rebase b0fced..f7ee6ab onto b0fced (4 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
```

At this point, you can identify the later commits as needing to be squashed (`s`) down into the oldest commit (the one you are `pick`ing):

```sh
pick 27fb3d2 Getting close.
s e8214df Missed a bug.
s ce5ed14 Uggggg! Why is this so hard?
s f7ee6ab Woot, finally got this working.
```

Once saved, `git` will prompt you to provide a cleaner commit message. And, once provided, your four shameful commits will be squashed down into a single, cohesive, meaningful commit.

### I want to squash several commits into one commit without using `rebase`.

In the vast majority of cases, if your `git` workflow is clean and true and your feature branch is short-lived, an interactive rebase should be straightforward and pain-free. However, once you _make the mistake_ of periodically merging `master` into your feature branch, you are inviting a world of hurt. In such a case, you can use the `merge` command with the `--squash` modifier as an escape hatch.

When you run `git merge --squash`, you copy the file-changes from one branch into another branch without actually copying the commit meta-data. Instead, the changes are brought-over as _staged changes_ on top of the current branch. At that point, you can commit all the staged changes as a single commit.

Assuming your `my-feature` branch needs to be "fixed", you can use the following workflow:

```sh
# Assuming that the `my-feature` branch is the branch that needs to be fixed,
# start off my renaming the branch as a backup (the `-m` modifier performs a
# rename):
git checkout my-feature
git branch -m my-feature-backup

# Now, checkout the `master` branch and use it to create a new, clean
# `my-feature` branch starting point.
git checkout master
git checkout -b my-feature

# At this point, your `my-feature` branch and your `master` branch should be
# identical. Now, you can "squash merge" your old feature branch into the new
# and clean `my-feature` branch:
git merge --squash my-feature-backup

# All the file-changes should now be in the `my-feature` branch as staged
# edits ready to be committed.
git commit -m "This feature is done and awesome."

# Delete the old backup branch as it is no longer needed. You will have to
# force delete (`-D`) it since it was never merged into `master`.
git branch -D my-feature-backup
```

> **ASIDE**: You should almost never need to do this. If you find yourself having to do this a lot; or, you find yourself dealing with a lot of "conflict resolution", you need to reevaluate your `git` workflow. Chances are, your feature branches are living way too long.

### I want to temporarily set-aside my feature work.

The life of a developer is typically "interrupt driven". As such, there is often a need to briefly set aside your current work in order to attend to more pressing matters. In such a case, it is _tempting_ to use `git stash` and `git stash pop` to store pending changes. But, _do not do this_. Stashing code requires unnecessary mental overhead. Instead, simply commit the changes to your current feature branch and then perform an interactive rebase later on in order to clean up your commits:

```sh
# Oh noes! Something urgent just came up - commit your changes to your feature
# branch and then go attend to the more pressing work.
git add .
git commit -m "Saving current work - jumping to more urgent matters."

git checkout master
```

Now, you never have to remember where those pending changes are. This guarantees that you won't lose your work.

If you were working directly on `master` when urgent matters came up, you can still avoid having to use `git stash`. To keep your work, simply checkout a new branch and the commit your pending changes to that new branch:

```sh
# Oh noes! Something urgent just came up - checkout a new branch. This will
# move all of your current work (staged and unstaged) over to the new branch.
git checkout -b temp-work

# Commit any unstaged changes.
git add .
git commit -m "Saving current work - jumping to more urgent matters."

git checkout master
```

Now, your `master` branch should be back in a pristine state and your `temp-work` branch can be continued later.

### I want to keep my changes during conflict resolution.

If your Git workflow is healthy - meaning that you have short-lived feature branches - conflicts should be very few and far between. In fact, I would assert that getting caught-up in frequent conflicts is an indication that something more fundamental to your workflow is primed for optimization.

That said, conflicts do happen. And, if you want to resolve a conflict by selecting "your version" of a file, you can use `git checkout --theirs` in a `merge` conflict, a `cherry-pick` conflict, and a `rebase` conflict.

In a `merge` conflict, `--theirs` indicates the branch being merged into the current context:

```sh
git checkout master
git merge --no-ff my-feature

# Oh noes! There is a conflict in "code.js". To keep your version of the
# code.js file, you can check-it-out using --theirs and the file path:
git checkout --theirs code.js
git add .
git merge --continue
```

Similarly, in a `cherry-pick` conflict, `--theirs` indicates the branch being cherry-picked into the current context:

```sh
git checkout master
git cherry-pick --no-ff my-feature

# Oh noes! There is a conflict in "code.js". To keep your version of the
# code.js file, you can check-it-out using --theirs and the file path:
git checkout --theirs code.js
git add .
git merge --continue
```

In a `rebase` conflict, `--theirs` indicates the branch that is being _replayed_ on top of the current context (**See Aside**):

```sh
git checkout my-feature
git rebase master

# Oh noes! There is a conflict in "code.js". To keep your version of the
# code.js file, you can check-it-out using --theirs and the file path:
git checkout --theirs code.js
git add .
git rebase --continue
```

> **ASIDE**: Using `--theirs` in a `rebase` can seem confusing because you are already in "your" feature branch. As such, it would seem logical that your version of the code would be targeted with `--ours`, not `--theirs`. However, a `rebase` operates _kind of like_ a series of `cherry-pick` operations. You can think of a `rebase` as doing the following:
> 
> * Check-out an earlier, common commit between your feature branch and the target branch.
> * Cherry-pick your feature branch commits onto the earlier commit.
> * Replace your feature branch with this temporary branch
> 
> With this mental model, "your" version - targeted using `--theirs` - is the version being cherry-picked into the "current context" (the temporary branch).
