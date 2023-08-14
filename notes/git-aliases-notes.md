---
title: "Useful Git aliases"
date: 2023-08-14
---

TODO: write as actual gitconfig - so could be copied

TODO: add description to each command

TODO: review function declarations (via f(){} instead of !sh or !bash?)

Here is the list of Git aliases that I found useful and have in my gitconfig (located under `/%User%/.gitconfig` on Windows):

```
[alias]
    #### TECH UTILITY COMMANDS ####
    # get name of main branch for this repository (as there could be 'main', 'master', 'develop', etc)
    main-branch = !git symbolic-ref refs/remotes/origin/HEAD | cut -d'/' -f4

    #### REPOSITORY STATE COMMANDS ####
    s = status
    lp = log --pretty=oneline

    #### CHECKOUT COMMANDS ####
    c = checkout
    cp = checkout -
    cb = "!sh -c \"git checkout -b $1\" -"
    cm = checkout master # replace master with main-branch?

    #### STASH COMMANDS ####
    sp = stash pop
    sc = stash clear

    #### OPERATIONS WITH REMOTE ####
    po = push origin HEAD
    pr = pull --rebase # review autostash option for pull

    #### GENERAL USEFUL COMMANDS ####
    # get all git aliases
    aliases = !bash -c \"git config --get-regexp alias\"

    # delete all branches but develop
    # replace develop with main-branch
    deletebranches = !bash -c \"git branch | grep -v \"develop\" | xargs git branch -D\"

    # ammend to previous commit without changing message
    cane = !bash -c \"git commit --amend --no-edit\"

    # Add alias for removing all files from staging
    https://stackoverflow.com/questions/19730565/how-to-remove-files-from-git-staging-area/19730687#19730687

    # squash all commits on branch since main-branch
    git switch yourBranch
    git reset --soft $(git merge-base master HEAD)
    git commit -m "one commit on yourBranch"

    # stash + checkout master + pull --rebase + go to previous branch + merge
    mm = "!sh -c \"git stash && git checkout master && git pull --rebase && git checkout - && git merge master\""

    # branch related
    bl = branch --list

    # review autostash option for pull

    https://stackoverflow.com/a/67672350/13608717
```
