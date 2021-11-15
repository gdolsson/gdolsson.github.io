---
layout: "post"
title: "Multiple Git Accounts"
date: "2021-11-15 19:14"
---
Say that you manage a personal and a work git account, how do you use multiple users from a single git computer?

Generate SSH keys (unless already available)

```bash
$ cd ~/.ssh
$ ssh-keygen -t rsa -b 4096 -C "your_personal_email@domain.com"
  # save as id_rsa_personal
$ ssh-keygen -t rsa -b 4096 -C "your_work_email@domain.com"
  # save as id_rsa_work
```

Add to GitHub account

```bash
$ pbcopy < ~/.ssh/id_rsa_personal.pub
```

Add the key(s) to the intended Git acount

```bash
$ cd ~/.ssh
$ vi config

    # Personal account - default config
    Host github.com-personal
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_rsa_personal
    # Work account
    Host github.com-work
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_rsa_work

$ cd ~
$ vi ~/.gitconfig

    [user]
        name = Personal Name
        email = your_personal_email@domain.com
    [includeIf "gitdir:~/work/"]
        path = ~/work/.gitconfig

$ vi ~/work/.gitconfig
	[user]
	    email = your_work_email@domain.com

$ ssh-add -D

$ ssh-add id_rsa_personal
$ ssh-add id_rsa_work

Validate
$ ssh-add -l

$ ssh -T github.com-personal
  # Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
$ ssh -T github.com-work
  # Hi USERNAME! You've successfully authenticated, but GitHub does   not provide shell access.

$ git clone git@github.com-personal:USERNAME/test-ssh.git 
$ cd test-ssh
$ touch index.html
$ echo "Hello World" >> index.html
$ git add .
$ git commit -m 'Add index file'
$ git push origin master
```

This should now allow you to pull using a specific identity and therefore also push to that repo as one user while also pulling and pushing to another account from the same computer.