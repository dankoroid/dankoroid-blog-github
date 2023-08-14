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
    # main-branch - get name of main branch for this repository (as there could be 'main', 'master', 'develop', etc)
    # main-branch call could be replaced with name of specific branch 'main', 'master', 'develop' etc if needed
    main-branch = !git symbolic-ref refs/remotes/origin/HEAD | cut -d'/' -f4

    #### REPOSITORY STATE COMMANDS ####
    s = status
    lp = log --pretty=oneline

    #### CHECKOUT COMMANDS ####
    c = checkout
    # cp - checkout to previous branch
    cp = checkout -
    # cb - create new branch & checkout to it (branch name should be provided as argument)
    cb = "!f() { git checkout -b \"$1\"; }; f"
    # cb - checkout to main branch
    cm = "!git checkout $(git main-branch)"

    #### STASH COMMANDS ####
    sp = stash pop
    sc = stash clear

    #### OPERATIONS WITH REMOTE ####
    # po - push current branch to origin with same branch name
    po = push origin HEAD
    # pr - pulls remote to local with rebase & also autoapplying 'stash' and 'stash pop'
    pr = pull --rebase --autostash

    #### GENERAL USEFUL COMMANDS ####
    # aliases - get all git aliases
    aliases = !git config --get-regexp '^alias\\.'

    # deletebranches - go to main-branch and delete all branches but main-branch
    deletebranches = !git checkout $(git main-branch) && git branch | grep -v \"$(git main-branch)\" | xargs git branch -D

    # cane - ammend to previous commit without changing message
    cane = commit --amend --no-edit

    # Add alias for removing all files from staging
    https://stackoverflow.com/questions/19730565/how-to-remove-files-from-git-staging-area/19730687#19730687

    # squash all commits on branch since main-branch
    git switch yourBranch
    git reset --soft $(git merge-base master HEAD)
    git commit -m "one commit on yourBranch"

    # mm - stash + checkout master + pull --rebase + go to previous branch + merge
    mm = "!sh -c \"git stash && git checkout master && git pull --rebase && git checkout - && git merge master\""

    # bl - branch related
    bl = branch --list

    # review autostash option for pull

    https://stackoverflow.com/a/67672350/13608717
```
