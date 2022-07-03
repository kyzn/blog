---
title: "Update GitHub Default Branches"
date: 2020-06-15T20:00:00Z
tags:
  - git
  - github
---

Each git repository comes with a default branch.  Here's a few-steps guide to update all your GitHub repositories' default branches.

## 1) Communicate

Changing the name of the default branch may be big for teams. If you have repos with many contributors, you should communicate first. Give them a timeline so they know what to expect.

## 2) Note that some features may not work

For the longest time master has been the default branch name in git and GitHub. So you may see some GitHub features not working properly.

One example is GitHub pages. It currently gives you two source options, and both are from the master branch. I think this will change soon. But in the meantime you may want to keep master branches of your GitHub pages repositories.

![](/images/github-pages-master.png)

## 3) Get ingydotnet/git-hub

[git-hub](https://github.com/ingydotnet/git-hub/) is a command line tool that you can use to talk to GitHub. There are three ways to install, all explained in its readme file. I will explain the preferred way here.

### 3.1) Clone git-hub repository

First step is to clone the repository. It's going to be run from the folder you cloned it to. I picked `/home/kivanc/.git-hub`, or `~/.git-hub` in short.

    git clone https://github.com/ingydotnet/git-hub ~/.git-hub

### 3.2) Run initialization script

Now we cloned the repository, but we still don't have `git hub` command available. We need to run `source` for initialization script.

    source ~/.git-hub/.rc

### 3.3) Run initialization script at startup

Instead of running `source` every time we need `git hub`, we can add it to our shell startup. I added it to `~/.bash_aliases`. If you don't have that, try `~/.bashrc`, `~/.bash_profile` or `~/.profile`. Note that you need two greater-than signs for "append". One greater-than means "overwrite".

    echo 'source ~/.git-hub/.rc' >> ~/.bash_aliases

### 3.4) Run git hub setup

For some git-hub commands to work, you need to run the following command. It will ask you for your GitHub username, password, and 2FA code (if any).

    git hub setup

Here's how it looks.

<center><script id="asciicast-339891" src="https://asciinema.org/a/339891.js" async></script></center>

## 4) Get ditch-master.sh

fREW wrote a script that automates the whole process. So you don't have to go through repos one by one. You can see the script at https://gist.github.com/frioux/6c259c63dda705f9ef9d030ddb880347.

You can download the script from there, or you can clone it as if it was a repository.

    git clone https://gist.github.com/6c259c63dda705f9ef9d030ddb880347.git ~/ditch-master

After you have the script, make sure it's executable:

    cd ~/ditch-master
    chmod a+x ditch-master.sh

Script will use "main" as default branch name. If you want to use something else, you should find and replace "main" with the word of your choosing. So for example, if you want to use "trunk", you can run the following.

    perl -i -pe 's/main/trunk/g' ditch-master.sh

All that is left is to run the script.

    ./ditch-master.sh

Here's how it looks.

<center><script id="asciicast-EqW4uYlihVExmzb0OiIDA4NUY" src="https://asciinema.org/a/EqW4uYlihVExmzb0OiIDA4NUY.js" async></script></center>

That's it!
