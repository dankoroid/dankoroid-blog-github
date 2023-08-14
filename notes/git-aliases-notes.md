---
title: "Useful Git aliases"
date: 2023-08-14
---

TODO: write as actual gitconfig - so could be copied

Here is the list of Git aliases that I found useful and have in my gitconfig (located under `/%User%/.gitconfig`):
- `s = status`
- `sp = stash pop`
- `sc = stash clear`
- `po = push origin HEAD`
- `pr = pull --rebase`
- `cp = checkout -` - this is checkout to previous branch
- `cb = "!sh -c \"git checkout -b $1\" -"` - checkout to new branch (new branch name should be passed as argument)
- `aliases = !bash -c \"git config --get-regexp alias\"`
- `cane = !bash -c \"git commit --amend --no-edit\"` - ammend to previous commit without changing message
- `deletebranches = !bash -c \"git branch | grep -v \"develop\" | xargs git branch -D\"` - double-check how to add multiple branches to be not deleted
- `` - Add alias for removing all files from staging
- `git switch yourBranch
git reset --soft $(git merge-base master HEAD)
git commit -m "one commit on yourBranch"` - how to squash all commits on branch (master is the branch that yourBranch diverged from) - https://stackoverflow.com/a/25357146
- ``
- ``

-----

git aliases:

[alias]
  # status related
  s = status

  # checkout related
  c = checkout
  cm = checkout master
  # checkout to previous branch
  cp = checkout -
  # checkout to new branch (new branch name should be passed as argument)
  cb = "!sh -c \"git checkout -b $1\" -"

  # stash related
  sp = stash pop
  sc = stash clear

  po = push origin HEAD
  pr = pull --rebase
  
  # branch related
  bl = branch --list

  # general
  # get all git aliases
  aliases = !bash -c \"git config --get-regexp alias\"
  
  # combined actions
  # checkout to master and pull --rebase
  cmpr = "!sh -c \"git checkout master && git pull --rebase\""
  # stash + checkout master + pull --rebase + go to previous branch + merge
  mm = "!sh -c \"git stash && git checkout master && git pull --rebase && git checkout - && git merge master\""
  # ammend to previous commit without changing message
  cane = !bash -c \"git commit --amend --no-edit\"
  # delete all branches but master/main
  deletebranches = !bash -c \"git branch | grep -v \"master$\" | grep -v \"main$\" | xargs git branch -D\"

------
------
https://stackoverflow.com/questions/19730565/how-to-remove-files-from-git-staging-area/19730687#19730687

------
https://ma.ttias.be/pretty-git-log-in-one-line/

------
https://stackoverflow.com/a/67672350/13608717

------
review autostash option for pull
