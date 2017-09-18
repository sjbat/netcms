---
layout: blog
title: Testowy wpis z Netlify
date: 2017-09-17T10:25:26+02:00
thumbnail: /images/lost-places.jpg
rating: '1'
---
Since I want to use git, the first thing I’ll need to do is install the git-core package (and its dependencies) and tell git who I am:
```
$ sudo apt-get -y install git-core
$ git config --global user.name "Jeremy L. Gaddis"
$ git config --global user.email jeremy@lab.evilrouters.net
```
Next, install etckeeper:
```
$ sudo apt-get -y install etckeeper
```
We’ll need to tell etckeeper that we want to use git instead of bzr. Open up /etc/etckeeper/etckeeper.conf in your favorite text editor. The first five lines look like this:
```
# The VCS to use.
# VCS="hg"
# VCS="git"
VCS="bzr"
# VCS="darcs"
```
We want it to look like this:
```
# The VCS to use.
# VCS="hg"
VCS="git"
# VCS="bzr"
# VCS="darcs"
```
With that change made, we can initialize the repository and do an initial commit to get things started:
```
$ sudo etckeeper init
Initialized empty Git repository in /etc/.git/
$ sudo etckeeper commit "Initial commit."
```
We could stop right here and be done. The Ubuntu etckeeper package sets up a cronjob that will run daily and auto-commit any changes to files in or under the /etc directory that we haven’t explicitly committed.

To check if there are any uncommitted changes, we can use the “git status” command:
```
$ cd /etc
$ sudo git status
# On branch master
nothing to commit (working directory) clean
“git log” will show us a history of commits:

$ sudo git log
commit b32d9b32f8c8cf9fe2a5d2070d6c08024bc497c5
Author: root 
Date:   Fri Feb 18 05:08:39 2011 -0500
Initial commit.
```
