---
id: 9m2k5vu5iwqtrpiwmmtx26l
title: Git
desc: ''
updated: 1663409164560
created: 1663409164560
isDir: false
title_imported: GIT
updated_imported: '2020-11-21T17:46:20.000Z'
created_imported: '2019-10-30T16:03:21.000Z'
---

[TOC]












------------


## [How to recover a dropped stash in Git?](https://stackoverflow.com/questions/89332/how-to-recover-a-dropped-stash-in-git#34666995)

```bash
git log --graph --decorate --pretty=oneline --abbrev-commit --all $(git fsck --no-reflogs | grep commit | cut -d' ' -f3)
```

You can do an even faster search if you're looking only for "WIP on" messages.

Once you know your sha1, you simply change your stash reflog to add the old stash :

```
git update-ref refs/stash ed6721d
```
You'll probably prefer to have an associated message so a -m
```
git update-ref -m "$(git log -1 --pretty=format:'%s' ed6721d)" refs/stash ed6721d
```

Or apply that
```
git stash apply $stash_hash
```


## [Gitmoji](https://gitmoji.carloscuesta.me/)

```bash

🎉 :tada: initial commit :tada:

🚀 :rocket: [Add] when implementing a new feature

🔨 :hammer: [Fix] when fixing a bug or issue

🎨 :art: [Refactor] when refactor/improving code

🚧 :construction: [WIP]

📝 :pencil: [MINOR] Some small updates

```
-----------------------------------



## Xóa branch mở nhầm
```
git branch -d <name-of-branch>
```

# update last commit without change the msg
```bash
git commit --amend --no-edit
```

# Make git log prettier

To make life easier, you can can add a git alias so you don’t have to remember the entire syntax.

```bash
git config --global alias.logline "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
git logline
```
[source](https://ma.ttias.be/pretty-git-log-in-one-line/)


![37c85eed3bbaaea62b1f37f0798efae6.png](/assets/37c85eed3bbaaea62b1f37f0798efae6-r1v3p1z8d0pq.png)

![6b0d1b2f9452d2b63cca96013f94db1c.png](/assets/6b0d1b2f9452d2b63cca96013f94db1c-kw0ysocqfker.png)

![f68a36b170ec38b919761ca5d92ecd6b.png](/assets/f68a36b170ec38b919761ca5d92ecd6b-ej1jnajtp7ph.png)



# 0. Configure tooling

Sets the name you want to attach to your commit transactions

```bash
$ git config --global user.name "[name]"

# Sets the email you want to attach to your commit transactions
$ git config --global user.email "[email address]" 

#CREATE REPOSITORIES
$ git init [project-name]  #Create a new local repository

# Create README.md file
```

# 1. Recording Changes to the Repository



```bash
git add [file] # Snapshots the file in preparation for committing them

git add -A  || git add --all # Add all the changed file 

git add . # Add all the changed file on the root folder
```
\
COMMIT CHANGES
```bash
git commit -m "[descriptive message]"
```
\
REDO CHANGES
```bash
git restore --staged <file>
git restore --staged . # restore all file out of stage
```
\
REMOVE FILES
```bash
git rm <file> # remove a file
```
\
RENAME FILE
```bash
git mv <file_from> <file_to> # rename a file by git
 
# submit a file renamed outside git
git mv <file_from>
git add <file_to>
``` 

# 2. Viewing Your Staged and Unstaged Changes
```bash
git status # Lists all new or modified files to be commited

git diff #shows you the exact lines added and removed in file
git diff --staged.
#This command compares your staged changes to your last commit:

git diff [first-branch]...[second-branch]
# Shows content differences between two branches
```

It’s important to note that git diff by itself doesn’t show all changes made since your last commit — only changes that are still unstaged.

If you’ve staged all of your changes, git diff will give you no output.
```bash
git log #Lists version history for the current branch
git log --oneline 
git log --follow [file] #Lists version history for a file, including renames
git show [commit] #Outputs metadata and content changes of the specified commit
```
# 3. Viewing the Commit History

For example, if someone else adds a few commits to the master branch of origin, you can do the following to download their changes and update your own local master branch:
```bash
 git checkout master      # Make sure you're on the right branch.
 git fetch                # Download any new info from origin.
 git merge origin/master  # Merge the 'origin/master' branch into your current branch.
```

# 4. Make a branch

```bash
 git checkout -b <new_branch> [<full Hash>] # comeback the old commit and make a branch

 git show-branch  # seeing the branches and their commits
```

With git you can work on a separate branch from the main line and make lots of small commits with their own messages. Although each of these commits by themselves may not solve the problem that you are working on, they do provide a way of saving the intermediate states of your work with the same sort of messages that you may have been updating in your commit message file.

Once you are ready to commit the sum of your work, you can use the `rebase` command and `squash` these commits together. Your editor will then be invoked with the all the individual messages that you used for the smaller commits which you can then edit together into a single message.

This is a lot easier than it sounds here, and is IMHO a more git-like approach.

# Rename a Git Branch – Local

Make sure you’re on the branch you want to rename:

 **1. Rename a Git branch with the -m command option**
The Git rename command will require you to ad the **-m** option to your command:

```bash
git branch -m new-name
```

**2. Rename a Git branch from another branch**
You can also rename a branch from another branch. The following example shows, how to rename a Git branch from the master branch:
```bash
git checkout master
git branch -m old-name new-name
```
 **3. Verify the process**

List all the branches to verify that the branch you want has been renamed:
```bash
git branch -a
```

# Git Stash TEMPORARY COMMITS

## Description
Use `git stash` when you want to record the current state of the working directory and the index, but want to go back to a clean working directory.

The command saves your local modifications away and reverts the working directory to match the `HEAD` commit.

The modifications stashed away by this command can be:
    + listed with `git stash list`
    + inspected with `git stash show`
    + restored (potentially on top of a different commit) with `git stash apply`.
    
Calling `git stash` without any arguments is equivalent to `git stash push`.

A `stash` is by default listed as "WIP on branchname …​", but you can give a more descriptive message on the command line when you create one.

The latest stash you created is stored in refs/stash; older stashes are found in the reflog of this reference and can be named using the usual reflog syntax (e.g. stash@{0} is the most recently created stash, stash@{1} is the one before it, stash@{2.hours.ago} is also possible). Stashes may also be referenced by specifying just the stash index (e.g. the integer n is equivalent to stash@{n}).

## Lưu lại thay đổi

Use git stash when you want to record the current state of the working directory and the index, but want to go back to a clean working directory.

It's useful when you want to change to another branch but it's unfinished on the current branch.

The command saves your local modifications away and reverts the working directory to match the HEAD commit.

```sh
git stash  #Save modified and staged changes
# equivalent to git stash push
```
\
Khi này branch đã trở nên "sạch sẽ" và `git status` sẽ cho thấy bạn có thể chuyển sang branch tuỳ thích.

Bạn có thể `git stash` bao nhiêu lần tuỳ thích và mỗi lần đó git sẽ lưu toàn bộ lần thay đổi đó như 1 phần tử trong 1 stack.

## Lấy lại thay đổi
Sau khi đã git stash 1 hoặc vài lần, bạn có thể xem lại danh sách các lần lưu thay đổi bằng câu lệnh

```sh
git stash list # list stack-order of stashed file changes

stash@{0}: WIP on <branch-name>: <lastest commit>
stash@{1}: WIP on <branch-name>: <lastest commit>
stash@{2}: WIP on <branch-name>: <lastest commit>

git stash list -p # Nếu muốn xem cả nội dung của từng thay đổi

git stash show stash@{1} #xem nội dung cụ thể hơn nữa của lần thay đổi thứ 1

git stash apply stash@{1} #Khi muốn apply lại thay đổi từ stash lần 1

```

## Xoá các thay đổi không cần thiết

Remove a single stashed state from the stash list and apply it on top of the current working tree state
The working directory must match the index.
```sh
git stash pop stash@{1} 
```

`apply` is like `pop` but do not remove the state from the stash list

```sh
git stash apply stash@{1}
```

Remove a single stash entry from the list of stash entries.
When no <stash> is given, it removes the latest one. i.e. stash@{0}

```sh
git stash drop stash@{1}
```




## Xóa toàn bộ stack
Those entries will then be subject to pruning, and may be impossible to recover

```sh
git stash clear #Remove all the stash entries
```

# Delete commits from a branch in Git
**Careful: `git reset --hard` WILL DELETE YOUR WORKING DIRECTORY CHANGES. Be sure to stash any local changes you want to keep before running this command.**

Assuming you are sitting on that commit, then this command will wack it...

```sh
git reset --hard HEAD~1
```
The HEAD~1 means the commit before head.

Or, you could look at the output of git log, find the commit id of the commit you want to back up to, and then do this:
```sh
git reset --hard <sha1-commit-id>

```
If you already pushed it, you will need to do a force push to get rid of it...
```sh
git push origin HEAD --force
```
However, if others may have pulled it, then you would be better off starting a new branch. Because when they pull, it will just merge it into their work, and you will get it pushed back up again.

If you already pushed, it may be better to use git revert, to create a "mirror image" commit that will undo the changes. However, both commits will be in the log.

FYI `git reset --hard HEAD` is great if you want to get rid of WORK IN PROGRESS. It will reset you back to the most recent commit, and erase all the changes in your working tree and index.

Lastly, if you need to find a commit that you "deleted", it is typically present in git reflog unless you have garbage collected your repository.

[@source](https://stackoverflow.com/questions/1338728/delete-commits-from-a-branch-in-git)

# Commit to the detached HEAD

The problem:

```bash
~/myProjects/Node/Jonas Course - Natour$ git status
HEAD detached from 799c077

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   .eslintrc.json
        modified:   .gitignore
        modified:   utils/appError.js
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        dev-data/requestUserApi.http

no changes added to commit (use "git add" and/or "git commit -a")

~/myProjects/Node/Jonas Course - Natour$ git checkout master

error: Your local changes to the following files would be overwritten by checkout:
        controller/authController.js
Please commit your changes or stash them before you switch branches.


~/myProjects/Node/Jonas Course - Natour$ git add -A
~/myProjects/Node/Jonas Course - Natour$ git commit -m 'signup login protect routes'

[detached HEAD 5dd389d] signup login protect routes
 18 files changed, 427 insertions(+), 105 deletions(-)
 rewrite controller/userController.js (81%)
 rewrite dev-data/authenticationApi.http (79%)
 create mode 100644 dev-data/requestUserApi.http
 
~/myProjects/Node/Jonas Course - Natour$ git status
HEAD detached from 799c077
nothing to commit, working tree clean

~/myProjects/Node/Jonas Course - Natour$ git checkout master

Warning: you are leaving 2 commits behind, not connected to
any of your branches:

  5dd389d signup login protect routes
  b4b8688 Modelling User - Create new user - encrypt password

If you want to keep them by creating a new branch, this may be a good time
to do so with:

 git branch <new-branch-name> 5dd389d
 
Switched to branch 'master'
 
```
The solution:

> if you type git reflog, it will show you the history of what revisions HEAD pointed to. Your detached head should be in there. Once you find it, do git checkout -b my-new-branch abc123 or git branch my-new-branch abc123 (where abc123 is the SHA-1 of the detached HEAD) to create a new branch that points to your detached head. Now you can merge that branch at your leisure. [@source](https://stackoverflow.com/questions/14757437/git-restore-last-detached-head)

```bash
~/myProjects/Node/Jonas Course - Natour$ git reflog

799c077 (HEAD) HEAD@{0}: checkout: moving from master to 799c077
fccf6f5 (master) HEAD@{1}: checkout: moving from 5dd389d9ee62630ab8623d09aaf47faf272f192e to master
5dd389d HEAD@{2}: commit: signup login protect routes


~/myProjects/Node/Jonas Course - Natour$ git checkout -b 'realone' 5dd389d9ee62630ab8623d09aaf47faf272f192e
Switched to a new branch 'realone'
```

Generally, if you check out a branch after working on a detached head, Git should tell you the commit from the detached head you had been on, so you can recover it if you need
```sh
~/myProjects/Node/Jonas Course - Natour$ git checkout master

Warning: you are leaving 2 commits behind, not connected to
any of your branches:

  5dd389d signup login protect routes
  b4b8688 Modelling User - Create new user - encrypt password

If you want to keep them by creating a new branch, this may be a good time
to do so with:

 git branch <new-branch-name> 5dd389d
 
Switched to branch 'master'
```

# Make the current Git branch a master branch
Make sure everything is pushed up to your remote repository (GitHub):

```
git checkout master
Overwrite "master" with "better_branch":

git reset --hard better_branch
Force the push to your remote repository:

git push -f origin master
```


# REAL LIFE EXAMPLE

Here’s a series of commands that could hypothetically be executed while developing a real feature.
See if you can figure out what they would each do, then try it out yourself and check.
```bash
 git clone https://github.com/cooperka/emoji-commit-messages.git
 cd emoji-commit-messages
 git status
 git checkout -b my-new-feature
 echo “This is a cool new file” > my-file.txt
 git status
 git add --all
 git status
 git diff HEAD
 git commit -m “Add my-file.txt”
 git status
 git log
 git push origin HEAD
 git checkout master
 git pull
```


# External Resource

## Cheat sheet
https://education.github.com/git-cheat-sheet-education.pdf

https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf

## Full Resource
https://git-scm.com/book/en/v2

## Guide
https://hackernoon.com/understanding-git-2-81feb12b8b26

## Specific topic
https://backlog.com/git-tutorial/undoing-changes/

https://git-scm.com/docs/git-stash
https://kipalog.com/posts/Su-dung-git-stash-hieu-qua

