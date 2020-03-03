# Git
## Branch
Reset master to origin/master: `git switch -C master origin/master`  
(~ delete the master branch, recreate at origin/master)

`git branch <new-branch>` create a new branch  
`git checkout -b <new-branch>` create a new branch and switch to it  
`git checkout -b <new-branch> <existing-branch>` create new branch based on existing branch (instead of HEAD)

## Make Changes
`git commit --amend` patch a commit

Rebase current branch on remote master:  
`git fetch origin`  
`git rebase origin/master`  
Conflict: solve, add files and `git rebase --continue`

`git merge <branch>` merge \<branch> into current branch  
`git rebase <branch>` rebase current branch onto \<branch>

## Switch Ref and Undo Things
### Commit-level
`HEAD~3` 3rd commits before current HEAD

`git checkout <commit>` move HEAD ref pointer to the commit  
`git checkout -b <new-branch>` create a new branch on the detached commit for further editing

`git revert HEAD` undo last commit by adding a new commit (can also undo older single commit)  

`git reset <commit>` move HEAD and branch ref to the chosen commit, undo all changes afterwards (Default option: `--mixed HEAD` -> unstage current changes)  
Options:  
`--soft` remove from commit history  
`--mixed` also remove from staged index  
`--hard` also remove from working directory

### File-level
`git reset <commit> <file>` update the file in stage index to match that in chosen commit (stage or unstage it), not update working directory

`git checkout <commit> <file>` update the file in working directory to match that in chosen commit -> often used with HEAD to discard all changes to a file

`git clean -f` remove all untracked files (not undo-able!)

`git rm <file>` remove tracked file on current branch from staging index and working directory (undo git rm: `git reset HEAD` or `git checkout .`, undo-able until a new commit is created)

`git stash`  
`git stash show` list stashed files  
`git stash apply` restore stashed files

## Vim
Editor opens after git commit: `i` to start entering text  
Quit the editor and commit: `esc` -> `:wq` -> `enter`

# Eclipse
Generate getter/setter: `Alt+Shift+S, R`

Go to definition: `F3`

Auto complete: 
- `Ctrl+Space`
- `Preference -> Java -> Editor -> Content assist` enter all characters to trigger: `abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ._`

## Debug (as Application)
`F5`: enter the call  
`F6`: go to next call  
`F7`: return to caller  
`F8`: resume execution

# Maven
`mvn clean install`  
`mvn spring-boot:run` (build and run)
`mvn clean package spring-boot:run`

# Network

## Security
**Authentication**: confirm user identity (username, password, ...)  
**Authorization**: allow access to some system resources (after authenticated)

WWW-Authenticate response header:  
includes authentication type and related information, received after sending request to a url without authorization

# Unit Testing
- **Dummy** object: passed around but never used (parameter list, ...)
- **Fake** object: with simplified implementation (in memery database instead of real database)
- **Stub** class: partial implementation for interface/class with predetermined behavior, record interactions for test
- **Mock** object: define output of certain method calls for interface/class, record interactions with the system for test

# Misc.
Win 10 environment variable: search `env`

Win 10 find the process listening to port 8080 and kill it:  
start cmd as admin: `netstat -ano | findStr "8080"`  
`taskkill /PID <pid> /F`

Cygwin navigate to C:\ `cd /cygdrive/c` or `cd c:`

