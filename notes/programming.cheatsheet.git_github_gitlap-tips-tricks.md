---
id: z8oat8gm515y1aq57bhlvwc
title: Git_github_gitlap Tips Tricks
desc: ''
updated: 1663409164560
created: 1663409164560
isDir: false
latitude: 10.8142
longitude: 106.6438
altitude: 0
title_imported: Git/GitHub/GitLap Tips Tricks
updated_imported: '2022-04-26T15:31:25.000Z'
created_imported: '2020-08-09T06:46:19.000Z'
---

[TOC]






























* * *

# Git Organized  

https://dev.to/render/git-organized-a-better-git-flow-56go

# HEAD và Detached HEAD

Khi quay ngược thời gian để thử version cũ hơn của dự án của bạn thì sao? Ví dụ: trong bối cảnh có lỗi, bạn muốn xem mọi thứ hoạt động như thế nào trong bản sửa đổi cũ hơn.

Đây là một trường hợp sử dụng hoàn toàn hợp lệ và phổ biến. Tuy nhiên, bạn không cần phải chuyển mình sang trạng thái Detached HEAD để đối phó với nó. Thay vào đó, hãy nhớ rằng toàn bộ khái niệm rẽ nhánh trong Git đơn giản và rẻ như thế nào: bạn có thể chỉ cần tạo một nhánh (tạm thời) và xóa nó sau khi hoàn tất.
```bash
$ git checkout -b test-branch 56a4e5c08

#...làm bất cứ thứ gì bạn muốn...

$ git checkout master
$ git branch -d test-branch
```

Làm thế nào để tìm lại một commit khi Detached HEAD?
Các bạn có thể dùng `git reflog` để xem lại log của HEAD ở đây bao gồm cả các commit ở trạng thái Detached HEAD. 

https://viblo.asia/p/biet-ve-head-va-detached-head-trong-git-de-tranh-bi-mat-code-m68Z04vMZkG

# `git rebase` editor using VS Code
```
git config --global core.editor "code --wait"
```
https://stackoverflow.com/questions/30024353/how-to-use-visual-studio-code-as-default-editor-for-git


# [`git pull --rebase` [Error] fatal: Cannot rebase onto multiple branches](https://stackoverflow.com/questions/35842492/git-cannot-rebase-onto-multiple-branches)

```bash
# fetch all the remote data
git fetch --all --prune
git pull --rebase --autostash # as normal
```


# [invalid upstream HEAD~2 - cannot rebase when master has only 2 branchs ](https://stackoverflow.com/questions/26174757/git-needed-a-single-revision-error/26174905#26174905)

In your case, there is no `HEAD~2`, since you only have 2 commits, hence the "`Needed a single revision`" error message.
Try:

```bash
  git rebase -i --root
```

see more about `--root` at "[Change first commit of project with Git?](https://stackoverflow.com/a/2309391/6309)"

# Quick change name email for recent commit
```bash
git commit --amend --author 'Vinh Nguyen <vinhnn261@gmail.com>'
```

# Change something (name, email, message) in old commit (not recent commit)

Becareful, it become a disaster if you try to edit some commit in remote, that someone had pull it.

For example, your git has two commits like below, and you want to edit something in the first commit, maybe change message, maybe change Author

```bash
git log

commit 0e3b102942c49153f6f9d48f40a4da2e27193b7e (HEAD -> master)
Author: Thinh Nguyen <maxiqboy@gmail.com>
Date:   Sat Jan 23 22:14:46 2021 +0700

    :twisted_rightwards_arrows: Change to use Row not Column to handle cards

commit d0105bcebda9a0490a4a44e55f3050e74d7c7cea
Author: Minh Pham <minhdpham@gmail.com>
Date:   Sat Jan 23 10:18:32 2021 +0700

    Initialize project using Create React App
``````bash
git rebase -i --root

# show screen like below

pick d0105bc Initialize project using Create React App
pick 0e3b102 :twisted_rightwards_arrows: Change to use Row not Column to handle cards

# Rebase 0e3b102 onto a1a2bcb (2 commands)
#
# Commands:
# p, pick <commit> = use commit

# r, reword <commit> = use commit, but edit the commit message

# e, edit <commit> = use commit, but stop for amending

# s, squash <commit> = use commit, but meld into previous commit

# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
```

We usually use :

- s for meld into previous commit
- e for change something like author, date

Edit `pick` in these line into some character we need,

```bash
e d0105bc Initialize project using Create React App
pick 0e3b102 :twisted_rightwards_arrows: Change to use Row not Column to handle cards
```

`Ctrl + S` to save `Ctrl + x` to leave that window

Now, it's time to make some changes. [Depending on the type of changes, you can perform the following if you need to change the:](https://confluence.atlassian.com/bitbucketserverkb/how-do-you-make-changes-on-a-specific-commit-779171729.html)

Không cần rebase

- The author of the commit `git commit --amend --author="Author Name <email@address.com>"`
    
- The date of the commit
    
    - For current date and time
        `git commit --amend --date="$(date -R)"`
        
    - For a specific date and time
        `{{git commit --amend --date="Thu, 07 Apr 2005 22:13:13 +0200"}}`
        (warning) Note that the Date format must follow the Git standard RFC 2822 or ISO 8601 which is described here
        
    - The commit message
        Perform: git commit --amend -m "New Commit Message"
        

After performing any of the above, a text editor will show up again. This is allow you to change the commit message if needed. Otherwise, just save it.

After make some change,
`git rebase --continue`

# Set author (name, email) riêng cho folder

```bash
git config user.name "Thinh Nguyen"
git config user.email thinh@gmail.com

#double-check to make sure everything ok
git config -l || git config --list

#edit the local git config
git config -e || git config --edit
```

# Khi tạo một nhánh `branchC` từ nhánh `branchA` nhưng đổi ý muốn nhánh đó phải được tạo từ nhánh `branchB`

Ta checkout sang nhánh `branchB` sao đó sử dụng

```
git checkout -B <branchC>
```

# Đặt lại author là tên mình khi làm việc trên máy người khác

Ta thêm option author vào commit như sau

```
git commit --author="Name <email>" -m "commit message"
Nếu đã nhỡ commit rồi muốn sửa lại thi ta có thể sử dụng

$ git commit --amend --author="Name <email>" [-m "commit message"]
```

# Turn back to the state before commit

```
git reset --soft HEAD~
```

# [Fatal: Not possible to fast-forward, aborting](https://stackoverflow.com/questions/13106179/fatal-not-possible-to-fast-forward-aborting#:~:text=3%20Answers&text=Your%20branch%20is%20no%20longer,completely%20contain%20the%20destination%20branch)

```js
git pull origin master --rebase 
```

# git lab tô cừn

GPhzJ2JpkdUfDvxhcNjd

# Muốn xóa hết những thay đổi chưa commit

```
git reset --hard #grhh
```

# Đặt sai tên branch

Đôi khi đặt tên một nơi mà code những thứ chẳng liên quan gì đến branch đó hay đặt sai chính tả

```
# Rename the local branch to the new name
git branch -m <old_name> <new_name>
```

## Khi bạn đã push lên remote thì làm sao?

```
# Delete the old branch on remote - where <remote> is, for example, origin
git push <remote> --delete <old_name>

# Or shorter way to delete remote branch [:]
git push <remote> :<old_name>

# Push the new branch to remote
git push <remote> <new_name>

# Reset the upstream branch for the new_name local branch
git push <remote> -u <new_name>
```

# Khi bạn viết nhầm và muốn thay đổi message của commit vừa tạo

```
$ git commit --amend
# Then, change commit message
```

# Khi lỡ commit và giờ muốn gỡ commit đó

```
git reset --soft HEAD^     # Use --soft if you want to keep your changes
git reset --hard HEAD^     # Use --hard if you don't care about keeping the changes you made
```

Note: Trường hợp sửa dụng git reset --soft HEAD^ trường hợp này rất tốt trong việc muốn gỡ 1 commit nhưng vẫn cần giữ lại nhưng thay đổi của commit đó, mình có thể thêm tiếp những gì muốn muốn thay đổi và tạo một commit mới.

Lưu ý nếu đã push lên và người khác đã pull về hay base trên nhánh của bạn thì không nên dùng cách trên mà hãy dùng `git revert <hash_commit>` nó sẽ tạo một commit xóa những gì bạn đã làm việc trên commit đó.

# Loại file ra khỏi commit vừa tạo

Nếu bạn muốn bỏ luôn commit vừa tạo sau đó thêm thay đổi và sẽ tạo commit mới sau thì hãy xem note thứ 3

Hoặc bạn có thể làm theo cách này

```
git reset --soft HEAD^ 

# or

git reset --soft HEAD~1

# Then reset the unwanted files in order to leave them out from the commit:
git reset HEAD path/to/unwanted_file

# Now commit again, you can even re-use the same commit message:
git commit -c ORIG_HEAD
```

Có thể bạn chưa biết ORIG_HEAD là gì hãy xem ở đây

Cách trên thì nguy hiểm quá, như này dễ hơn nè =))

```
git reset HEAD^ -- path/to/file
git commit --amend --no-edit
```

# Khi bạn commit nhầm vào một nhánh khác

Nếu chỉ có 1 commit ta có thể làm theo cách này:

```
# Get commit hash 

# Switched to destination branch
git checkout destination_branch

$ git cherry-pick <commit_hash>
```

Cách này không tốt ở chỗ ở nhánh commit nhầm vẫn còn commit đó, vì vậy cần mắc công quay lại xóa commit đó. Cách làm thế nào các bạn có thể xem bên trên

# Chuyển các commit từ branch này sang branch khác

Vậy với trường hợp thế này thì sao?

```
master A - B - C - D - E

->

newbranch     C - D - E
             /
master A - B 
```

Trường hợp đã có nhánh đó rồi

```
git checkout existingbranch
git merge master         # Bring the commits here
git checkout master
git reset --keep HEAD~3  # Move master back by 3 commits.
git checkout existingbranch
```

Trường hợp muốn cắt 3 commit kia sang nhánh mới

```
git branch newbranch      # Create a new branch, containing all current commits
git reset --keep HEAD~3   # Move master back by 3 commits (Make sure you know how many commits you need to go back)
git checkout newbranch    # Go to the new branch that still has the desired commits
# Warning: after this it's not safe to do a rebase in newbranch without extra care.
```

Như phía trên ta sử dụng cherry-pick để chuyển 1 commit vậy ta có thể áp dụng với nhiều commit được không?

Tất nhiên là có rồi ta có thể thực hiện n lần cherry-pick cơ mà.

Tuy nhiên ai lại làm thủ công thế, vì cherry-pick ta có thể sử dụng ranges(từ phiên bản Git 1.7.2)

Giả sử ta ta có

```
C commit: 9aa1233
D commit: 453ac3d
E commit: 612ecb3

```

Thì ta có thể sử dụng

```
git checkout newbranch
git cherry-pick 612ecb3~1..9aa1233

# git cherry-pick applies those three commits to newbranch.
```

# Nhỡ tay xóa commit và muốn khôi phục lại

Khi bạn nhỡ tay git reset --hard HEAD~1 và phát hiện ngay thì chỉ cần sử dụng ngay git reset --hard HEAD@{1}

Còn không thì hãy bình tĩnh xem lịch sử các commit bằng lệnh git reflog Sau đó tìm commit muốn phục và khôi phục bằng lệnh git reset --hard &lt;commit_hash&gt;

# Khi lỡ tay xoá mất branch và muốn lấy lại

Khi bạn đã commit rồi thì chứ yên tâm sẽ chẳng bao giờ có thể mất được code đâu chứ bình tĩnh =))

Đầu tiên hãy xem lại hết lịch sử commit bằng cách `git reflog` hãy tìm commit bạn đã commit ở branch bạn xóa, sau đó sử dụng `git branch <new_branch> <commit_hash>` ta có thể thay commit hash bằng HEAD@{n}

# Sau khi merge mà không tự tin lắm muốn trở lại trước lúc merge

`git reset --hard ORIG_HEAD` Cách trên cũng áp dụng được với rebase , hoặc không thì ta có lại sử dụng đến `git reflog` sau đó `git reset --head <commit_hash>` để tìm lại commit cuối cùng của branch trước khi merge hoặc rebase

Trong khi merge hay rebase nếu thấy không ổn hơi nhiều mà chán quá không muốn nữa thì dùng `git merge --abort` hay `git rebase -- abort` các bạn nhé.

# Gộp các commit trong một branch và merge vào một nhánh khác

Ví dụ merge branch issue gồm nhiều commit vào branch master

```
# Switched to branch 'master'
$ git checkout master
$ git merge --squash issue
```

Lệnh này giống với cái nút merge trong option squash trên github web

# Nhỡ pull về mà conflic nhiều quá, fix không nổi nản quá muốn trở lại =))

Sử dụng lệnh pull `git pull origin master` Sau đó conflic nhiều muốn trở lại bạn có thể dùng 1 trong 2 lệnh

git merge --abort or git reset --hard ORIG_HEAD

# Khi bạn muốn change base

```
(commit 1) - master
               \-- (commit 2) - (commit 3) - demo

#Và bạn muốn change base nhánh PRO từ demo sang master

(commit 1) - master
               |-- (commit 2) - (commit 3) - demo
               \-- (commit 4) - (commit 5) - PRO
```

Ta có thể dễ dàng làm việc này với lệnh này

`git rebase --onto newBase oldBase feature/branch`

Với ví dụ trên ta sẽ làm thế này

```
git checkout PRO # Just to be clear which branch to be on.
git rebase --onto master demo PRO
```

Lệnh này cũng rất hữu dụng khi bạn làm việc khi bạn phải base trên nhánh đang phát triển của một người khác(nhánh A), và nhánh đó sau đó được merge vào 1 nhánh khác (nhánh B), bạn sẽ phải đổi base từ nhánh A sang nhánh B.