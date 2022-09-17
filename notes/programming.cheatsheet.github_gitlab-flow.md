---
id: jbyaepea49gcincqhzs9uaw
title: Github_gitlab Flow
desc: ''
updated: 1663409164560
created: 1663409164560
isDir: false
latitude: 21.0278
longitude: 105.834
altitude: 0
title_imported: GitHub/GitLab Flow
updated_imported: '2022-03-22T17:36:34.000Z'
created_imported: '2020-01-03T12:20:49.000Z'
---

[TOC]


# [Multiple SSH key in one machine](https://stackoverflow.com/questions/2419566/best-way-to-use-multiple-ssh-private-keys-on-one-client)

## 1. Generate an SSH key:ònionf
```
$ ssh-keygen -t rsa -C <email1@example.com>
```


## 2. Generate another SSH key:
```
$ ssh-keygen -t rsa -f ~/.ssh/accountB -C <email2@example.com>
```
Now, two public keys (id_rsa.pub, accountB.pub) should be exists in the ~/.ssh/ directory.
```
$ ls -l ~/.ssh     # see the files of '~/.ssh/' directory
```


## 3. Create configuration file ~/.ssh/config with the following contents:
```
$ code ~/.ssh/config
```

Content
```bash
Host bastian
    User git
    Hostname gitlab.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_ed25519

Host maxiqboy
    User git
    Hostname github.com
    PreferredAuthentications publickey
    IdentitiesOnly yes
    IdentityFile ~/.ssh/maxiqboy/id_rsa
```

```bash
Host glab
    User git
    Hostname gitlab.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_ed25519

Host anglehub
    User git
    Hostname github.com
    PreferredAuthentications publickey
    IdentitiesOnly yes
    IdentityFile ~/.ssh/id_anglehub_rsa
    
Host ghub
    User git
    Hostname github.com
    PreferredAuthentications publickey
    IdentitiesOnly yes
    IdentityFile ~/.ssh/id_rsa
	
Host mudah
    User git
    Hostname gitlab2.mudah.my
    PreferredAuthentications publickey
    IdentitiesOnly yes
    IdentityFile ~/.ssh/id_rsa
	
Host mudah
    User git
    Hostname gitlab2.mudah.my
    Port 24122
    PreferredAuthentications publickey
    IdentitiesOnly yes
    IdentityFile ~/.ssh/id_rsa
```

## 4. Test connection
```bash
ssh -T git@bastian                                                               
"Welcome to GitLab, @minhdpham0603!"

ssh -T git@maxiqboy                                                             
"Hi thinh105! You've successfully authenticated, but GitHub does not provide shell access."

# without port
ssh -T git@mudah         
ssh: connect to host gitlab2.mudah.my port 22: No route to host

# without port
ssh -T git@mudah -p 24122
Welcome to GitLab, @thinhnv_cmcglobal!

# with port
ssh -T git@mudah
Welcome to GitLab, @thinhnv_cmcglobal!

```

Some time, we got some errors:

![e4d5e8568325a813cb0864ee596f34aa.png](/assets/e4d5e8568325a813cb0864ee596f34aa-j1k8bxbr89g4.png)

```bash
OpenSSH_8.2p1 Ubuntu-4ubuntu0.4, OpenSSL 1.1.1f  31 Mar 2020
debug1: Reading configuration data /home/bastian/.ssh/config
debug1: /home/bastian/.ssh/config line 2: Applying options for mudah
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: include /etc/ssh/ssh_config.d/*.conf matched no files
debug1: /etc/ssh/ssh_config line 21: Applying options for *
debug1: Connecting to gitlab2.mudah.my [103.10.41.109] port 24122.
debug1: Connection established.
debug1: identity file /home/bastian/.ssh/id_ed25519_mudah type 3
debug1: identity file /home/bastian/.ssh/id_ed25519_mudah-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.4
debug1: Remote protocol version 2.0, remote software version OpenSSH_5.3
debug1: match: OpenSSH_5.3 pat OpenSSH_5* compat 0x0c000002
debug1: Authenticating to gitlab2.mudah.my:24122 as 'git'
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: diffie-hellman-group-exchange-sha256
debug1: kex: host key algorithm: ssh-rsa
debug1: kex: server->client cipher: aes128-ctr MAC: umac-64@openssh.com compression: none
debug1: kex: client->server cipher: aes128-ctr MAC: umac-64@openssh.com compression: none
debug1: SSH2_MSG_KEX_DH_GEX_REQUEST(2048<3072<8192) sent
debug1: got SSH2_MSG_KEX_DH_GEX_GROUP
debug1: SSH2_MSG_KEX_DH_GEX_INIT sent
debug1: got SSH2_MSG_KEX_DH_GEX_REPLY
debug1: Server host key: ssh-rsa SHA256:an3QHivUlWqfreevVwEWzhqzow9up4G4h3KCE3+iVUU
debug1: Host '[gitlab2.mudah.my]:24122' is known and matches the RSA host key.
debug1: Found key in /home/bastian/.ssh/known_hosts:1
debug1: rekey out after 4294967296 blocks
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: SSH2_MSG_NEWKEYS received
debug1: rekey in after 4294967296 blocks
debug1: Will attempt key: /home/bastian/.ssh/id_ed25519_mudah ED25519 SHA256:VXqP+ZOr1SY8tBHRStnwf6Zf+ObApmu/txTIqH7NatM explicit agent
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey,password
debug1: Next authentication method: publickey
debug1: Offering public key: /home/bastian/.ssh/id_ed25519_mudah ED25519 SHA256:VXqP+ZOr1SY8tBHRStnwf6Zf+ObApmu/txTIqH7NatM explicit agent
debug1: Authentications that can continue: publickey,password
debug1: No more authentication methods to try.
git@gitlab2.mudah.my: Permission denied (publickey,password).
```


Turn out, our Server is too old and not support the ED25519 ssh key
![7e44929de383311d6ee9fc361a443a7e.png](/assets/7e44929de383311d6ee9fc361a443a7e-ed0059wasri4.png)

![b6d25b61101ffa5b6ece21b34382dfa8.png](/assets/b6d25b61101ffa5b6ece21b34382dfa8-evknoy7x7zgg.png)

We just create a RSA key, and everything is fine

![b80d441d16093848658979027298b8c9.png](/assets/b80d441d16093848658979027298b8c9-dw2epmsi9di2.png)

## 5. Clone git

Now, when you are cloning the repository (named demo) from the company's GitLab account, the repository URL will be something like: Repo URL: `git@github.com:abc1234/demo.git`

Now, while doing git clone, you should modify the above repository URL as: `git@bastian:abc1234/demo.git`

Notice how github.com is now replaced with the alias "bastian" as we have defined in the configuration file.

From
```
ssh://git@gitlab2.mudah.my:24122/mudah-code/mobile-revamp.git
```

```
git clone ssh://git@mudah/mudah-code/mobile-revamp.git

git clone ssh://git@mudah:24122/mudah-code/mobile-revamp.git 
```


## 6. Modified the local git config in each folder

Similary, you have to modify the clone URL of the repository in the personal account depending upon the alias provided in the configuration file.

```bash
git remote add origin git@maxiqboy:thinh105/rename-multiple-files.git

# push changes to github
git push -u origin master || git push --set-upstream origin master
```

## 7 Set Username user email
```
git config user.email "thinhnv@cmcglobal.vn"
git config user.name "Thinh Nguyen"
```


# [Set Prune on Fetch](https://spin.atomicobject.com/2020/05/05/git-configurations-default/)

This configuration will automatically clean Git objects in your repository locally whenever you fetch changes from remote.

```
git config --global fetch.prune true
```

If this configuration is set, running git fetch will also run git remote prune afterwards. git remote prune will delete inaccessible Git objects in your local repository that aren’t on remote. Deleting branches on remote but not locally will generate these inaccessible Git objects.

Having this option enabled minimizes the number of branches I have on my local machine. Any autocomplete feature that uses this list of branches is much easier to use with limited branches hanging around.


# [Set git pull autostack and rebase](https://cscheng.info/2017/01/26/git-tip-autostash-with-git-pull-rebase.html)

```
git pull --rebase
```

If there are uncommitted changes, you need to stash those changes first, then pull the remote updates, and pop your stash to continue your work. This gets quite tedious, but since Git 2.6 you can use the autostash option to automatically stash and pop your uncommitted changes.

```
git pull --rebase --autostash
```

Instead of invoking this option manually, you can also set this for your repository with git config:

```
$ git config pull.rebase true
$ git config rebase.autoStash true
```

Or you can set this globally for every Git repository:
```
$ git config --global pull.rebase true
$ git config --global rebase.autoStash true
```

## checkout remote branch
```bash
git checkout "name-on-remote-not-contain-origin"
```

## Git rebase Workflow
```bash
# Checkout a new working branch
 git checkout -b <branchname>
 
# Make Changes
 git add -A
 git commit -m "description of changes"
 
# Sync with remote
 git checkout master
 git pull --rebase
 
# Update branch
 git checkout <branchname>
 git rebase master
 
# Push Changes
 git checkout master
 git merge <branchname>
 git push

# Squash Last N Commits
 git rebase --interactive HEAD~N

# Conflicts
# Resolve conflict my looking at the files in question.
git add
git rebase --continue

# Fetch changes from all remotes and locally delete 
# remote deleted branches/tags etc
# --prune will do the job :-;
git fetch --all --prune
```

# Squash Last N Commits
```js
git rebase --interactive HEAD~N
```

![ffb5185825d5c7ffd692519647fd2c07.png](/assets/ffb5185825d5c7ffd692519647fd2c07-cf3ju5r8hx1f.png)

## Create a pull request in GitHub

```bash

# 1. Fork it
# When you want to work on a GitHub project, the first step is to fork a repo.

# 2. Clone it to your local system
git clone https://github.com/<YourUserName>/demo

# 3. Make a new branch
git checkout -b new_branch

# 4. Make your changes


# 5. Push it back to your repo
git push -u origin new_branch

# 6. Click the `Compare & pull` request button

# 7.  Click `Create pull request` to open a new pull request

```

## **Contribute to an existing repository**

```bash
# download a repository on GitHub.com to our machine
git clone https://github.com/me/repo.git

# change into the `repo` directory
cd repo

# create a new branch to store any new changes
git branch my-branch

# switch to that branch (line of development)
git checkout my-branch

# make changes, for example, edit `file1.md` and `file2.md` using the text editor

# stage the changed files
git add file1.md file2.md

# take a snapshot of the staging area (anything that's been added)
git commit -m "my snapshot"

# push changes to github
git push --set-upstream origin my-branch
```

## **Start a new repository and publish it to GitHub**

First, you will need to create a new repository on GitHub.

Do not initialize the repository with a README, .gitignore or License.

This empty repository will await your code.

```bash
# create a new directory, and initialize it
# with git-specific functions
git init my-repo

# change into the `my-repo` directory
cd my-repo

# create the first file in the project
touch README.md

# git isn't aware of the file, stage it
git add README.md

# take a snapshot of the staging area
git commit -m "add README to initial commit"

# provide the path for the repository you created on github
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
# or
git remote add origin git@github.com:maxiqboy/VueJS-calculator.git

# push changes to github
git push --set-upstream origin master
# or if you in branch beta on local
git push --set-upstream origin beta 

```

## **Contribute to an existing branch on GitHub**

```bash
# assumption: a project called `repo` already exists on the machine,
# and a new branch has been pushed to GitHub.com
# since the last time changes were made locally

# change into the `repo` directory
cd repo

# update all remote tracking branches,
# and the currently checked out branch
git pull

# change into the existing branch called `feature-a`
git checkout feature-a

# make changes, for example, edit `file1.md` using the text editor

# stage the changed file
git add file1.md

# take a snapshot of the staging area
git commit -m "edit file1"

# push changes to github
git push
```

## GitHub flow
https://guides.github.com/introduction/flow/

[@source](https://guides.github.com/introduction/git-handbook/)

https://viblo.asia/p/mot-vai-kho-khan-sai-lam-su-co-voi-git-toi-tung-gap-phai-va-toi-da-giai-quyet-no-the-nao-gAm5y4eDldb?fbclid=IwAR1hngysPOhk1qtN5oFAMn9w0xAMvjCdsSqEN2lXkWoYgBjnzTGFp9UuWfA#_tong-ket-14

## Tạo SSH key - Generating a new SSH key

Open Terminal. Paste the text below, substituting in your GitHub email address.
```sh
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
This creates a new ssh key, using the provided email as a label.


```sh
> Generating public/private rsa key pair.
```

When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.

```sh
> Enter a file in which to save the key (/home/you/.ssh/id_rsa): [Press enter]
```
At the prompt, type a secure passphrase. For more information, see "Working with SSH key passphrases".
```sh
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```

\
**Result**
```sh
~$ ssh-keygen -t rsa -b 4096 -C "maxiXXXX@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/bastieng/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/bastieng/.ssh/id_rsa.
Your public key has been saved in /home/bastieng/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:ac7iTGaArZzSIgAAAAAAAASVpKy+NAQQOV0rUvg maAAAboy@gmail.com
The key's randomart image is:
+---[RSA 4096]----+
|o+O*..   .       |
|.==.. o E .      |
|+o+* + E E       |
|.o+E* o ..       |
| .o o+ .S        |
|.o oo. +         |
|+ =  += o        |
|.o  .*o.         |
|     .o          |
+----[SHA256]-----+

```

## Testing SSH connection

```sh
~$ ssh -T git@github.com
The authenticity of host 'github.com (13.XXX.188.59)' can't be established.
RSA key fingerprint is SHA256:nThbg6XXXXGOCspRomTxXXXXXXE5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,13.XXX.188.59' (RSA) to the list of known hosts.
Enter passphrase for key '/home/bastien/.ssh/id_rsa': 
Hi maxiqboyss! You've successfully authenticated, but GitHub does not provide shell access.

```

https://help.github.com/en/github/authenticating-to-github/testing-your-ssh-connection

## Add ssh key to GitHub
https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account

@source:
https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh

https://huyhoang17.github.io/git/2017/04/21/introduction-to-git.html

