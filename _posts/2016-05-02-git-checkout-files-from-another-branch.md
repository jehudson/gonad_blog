---
layout: post
title: "Git Checkout files from another branch"
excerpt: The Git checkout command can be used to update specific files or directories in your current tree from another branch.
categories: [git]
comments: true
---

The git-checkout command can be used to update specific files or directories in your working tree with those from another branch, without merging in the whole branch. This can be useful when working with several feature branches.

The [git checkout manual page](http://schacon.github.com/git/git-checkout.html) describes how the git checkout command
is not just useful for switching between branches.

In order to update the working tree with files or directories from another branch, you can use the branch name pointer in the git checkout command.

```bash
git checkout <branch_name> -- <paths>
```

As an example, this is how you could update your develop branch  to include the latest changes made to a file that is on the master branch.

```bash
# On branch master
git checkout develop-branch
git checkout master -- myplugin.js
git commit -m "Update myplugin.js from master"
```
