---
**Write down my Git bash configuration**

Changing what is displayed on git line

go to /Git/etc/profile.d/git-prompt.sh

add next lines
> PS1="$PS1"'\t '                # display current time in 24-hour HH:MM:SS format
> 
> PS1="$PS1"'(\d) '            # display current date in "Weekday Month Date" format (e.g., "Tue May 26") 

---
**Write down my Git configuration**

> git config --global merge.conflictstyle diff3

> git config --global help.autocorrect 50

> git config --global status.relativePaths false

> git config --global status.short true

---
Write down my Git aliases

go to /%User%/.gitconfig

under [alias] add
- s = status
- sp = stash pop
- sc = stash clear
- po = push origin HEAD
- pr = pull --rebase
- cp = checkout -
this is checkout to previous branch
- cb = "!sh -c \"git checkout -b $1\" -"
checkout to new branch (new branch name should be passed as argument)
- aliases = !bash -c \"git config --get-regexp alias\"
- cane = !bash -c \"git commit --amend --no-edit\"
ammend to previous commit without changing message
- deletebranches = !bash -c \"git branch | grep -v \"develop\" | xargs git branch -D\"
double-check how to add multiple branches to be not deleted

Add alias for removing all files from staging



---
Collect all my bookmarks on Git and share here

---
