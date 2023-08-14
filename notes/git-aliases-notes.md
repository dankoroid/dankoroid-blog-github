---
title: "Useful Git aliases"
date: 2023-08-14
---

Here is the list of Git aliases that I found useful and have in my gitconfig (located under `/%User%/.gitconfig` on Windows):

```
[alias]
    #### TECH UTILITY COMMANDS ####
    # main-branch - get name of main branch for this repository (as there could be 'main', 'master', 'develop', etc)
    # main-branch call could be replaced with name of specific branch 'main', 'master', 'develop' etc if needed
    main-branch = !git symbolic-ref refs/remotes/origin/HEAD | cut -d'/' -f4

    #### REPOSITORY STATE COMMANDS ####
    s = status
    # lp - pretty log last N commits (pass value as argument or default to 25)
    lp = "!f() { git log --pretty=oneline -n ${1:-25}; }; f"
    # bl - get all branches
    bl = branch --list

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

    # unstage - remove all files from staging
    unstage = reset HEAD --

    # squash all commits on branch since main-branch
    squash-since-main = "!f() { \
        current_branch=HEAD; \
        main_branch=$(git main_branch); \
        base_commit=$(git merge-base $current_branch $main_branch); \
        git reset --soft $base_commit; \
        git commit -m \"$1\"; \
    }; f"

    # mm - stash + checkout master + pull --rebase + go to previous branch + merge
    mm = "!f() { \
        git stash; \
        git cm; \
        git pull --rebase; \
        git checkout -; \
        git merge $(git main-branch); \
    }; f"
```
