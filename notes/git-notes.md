------
**Write down my Git bash configuration**

Changing what is displayed on git line

go to /Git/etc/profile.d/git-prompt.sh

add next lines
> PS1="$PS1"'\t '                # display current time in 24-hour HH:MM:SS format
> 
> PS1="$PS1"'(\d) '            # display current date in "Weekday Month Date" format (e.g., "Tue May 26")

------
if test -f ~/.config/git/git-prompt.sh
then
  . ~/.config/git/git-prompt.sh
else
  PS1='\[\033]0;$TITLEPREFIX:$PWD\007\]' # set window title
  PS1="$PS1"'\n'                 # new line
  PS1="$PS1"'\[\033[31m\]'       # change to red
  PS1="$PS1"'\t '                # time
  PS1="$PS1"'\[\033[32m\]'       # change to green
  PS1="$PS1"'\u@\h '             # user@host<space>
  PS1="$PS1"'\[\033[35m\]'       # change to purple
  PS1="$PS1"'$MSYSTEM '          # show MSYSTEM
  PS1="$PS1"'\[\033[33m\]'       # change to brownish yellow
  PS1="$PS1"'\w'                 # current working directory
  if test -z "$WINELOADERNOEXEC"
  then
    GIT_EXEC_PATH="$(git --exec-path 2>/dev/null)"
    COMPLETION_PATH="${GIT_EXEC_PATH%/libexec/git-core}"
    COMPLETION_PATH="${COMPLETION_PATH%/lib/git-core}"
    COMPLETION_PATH="$COMPLETION_PATH/share/git/completion"
    if test -f "$COMPLETION_PATH/git-prompt.sh"
    then
      . "$COMPLETION_PATH/git-completion.bash"
      . "$COMPLETION_PATH/git-prompt.sh"
      PS1="$PS1"'\[\033[36m\]'  # change color to cyan
      PS1="$PS1"'__git_ps1'   # bash function
    fi
  fi
  PS1="$PS1"'\[\033[0m\]'        # change color
  PS1="$PS1"'\n'                 # new line
  PS1="$PS1"'$ '                 # prompt: always $
fi

------
**Write down my Git configuration**

> git config --global merge.conflictstyle diff3

> git config --global help.autocorrect 50

> git config --global status.relativePaths false

> git config --global status.short true

git config:

[help]
  # will correct to nearest operation in N/10 seconds. 50 = 5s
  autocorrect = 50

[status]
  relativePaths = false
  # really short info in git bash
  short = true

------
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

```
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
```

------
review these git-related links:

https://superuser.com/questions/1023492/how-can-i-configure-git-bash-to-display-a-timestamp-for-each-command
https://manpages.ubuntu.com/manpages/kinetic/en/man3/strftime.3avr.html
https://bneijt.nl/blog/add-a-timestamp-to-your-bash-prompt/
https://misc.flogisoft.com/bash/tip_colors_and_formatting

https://ma.ttias.be/pretty-git-log-in-one-line/

https://stackoverflow.com/questions/25356810/git-how-to-squash-all-commits-on-branch/25357146#25357146

https://stackoverflow.com/questions/70116879/how-to-delete-all-local-branches-except-master-and-develop-in-one-command-withou/70117111#70117111

https://stackoverflow.com/questions/19730565/how-to-remove-files-from-git-staging-area/19730687#19730687

------
how to squash all commits on branch:

git switch yourBranch
git reset --soft $(git merge-base master HEAD)
git commit -m "one commit on yourBranch"

notes:
master is the branch that yourBranch diverged from

https://stackoverflow.com/a/25357146

------
