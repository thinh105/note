---
id: 1hy1wozz5ohmoj8azuh2wl7
title: Git
desc: ''
updated: 1663409164556
created: 1663409164556
isDir: false
title_imported: Git
updated_imported: '2022-01-12T03:46:10.000Z'
created_imported: '2019-10-30T16:03:21.000Z'
---

# **Git workflow**

https://sandofsky.com/workflow/git-workflow/

**There are three main components of a Git project:**

- Repository

    The repository, or repo, is the “container” that tracks the changes to your project files. It holds all the commits—a snapshot of all your files at a point in time—that have been made. You can access the commit history with the Git log.

- Working tree

    The working tree, or working directory, consists of files that you are currently working on. You can think of a working tree as a file system where you can view and modify files.

- Index

    The index, or staging area, is where commits are prepared. The index compares the files in the working tree to the files in the repo. When you make a change in the working tree, the index marks the file as modified before it is committed.

![](https://backlog.com/app/themes/backlog-child/assets/img/guides/git/basics/git_workflow_001.png)

**Below is the basic Git workflow:**

1. Modify files in the working tree.
2. Stage the changes you want to be included in the next commit.
3. Commit changes. Committing will take the files from the index and store them as a snapshot in the repository.

## **Three states of Git files**

- Modified
- Staged
- Committed

1. When a file is first modified, the change can only be found in the working tree.
2. You must stage the changes you want to be included in your next commit.
3. The index contains all file changes that will be committed.
4. Once you have finished staging files, commit them with a message describing what you changed. The modified files are now safely stored in the repo.

![](https://backlog.com/app/themes/backlog-child/assets/img/guides/git/basics/git_workflow_002.png)

The three file states for Git: modified, staged, and commited.
![](http://i.imgur.com/ieOhAOg.png)

After editing some files, these commands will mark any changes you’ve made as “staged” (or “ready to be committed”).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIwNjgyNDUwNV19
-->