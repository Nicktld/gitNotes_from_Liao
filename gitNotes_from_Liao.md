# <p align = "center"> Git Notes </p>
## Introduction
- **Git** is a distributed version control system.
- **SVN** vs **Git**, Centralized or Distributed?  
  - **SVN** has a central repository to store all files and historical data.  
  **Git** has a central repository as well as a series of local repositories. Local repositories are exact copies of the central repository complete with the entire history of changes.

  - **SVN** developers commit their changes directly to that central server repository. Offline access is limited.  
  **Git** developers work on their local repositories and then merge into the central repository, allowing them to continue working without losing features if they lose conncetion.

  - **SVN** may have a single point of failure. If there is an error, it can destory all builds. When the central repository is broken, developers have to wait until the central repository is fixed.  
  **Git** developers can continue to commit code locally until the central repository is fixed.

## Git Commands
### Git Configuration
    $ git config --global user.name "Your Name"
    $ git config --global user.email "email@example.com"

The parameter `--global` indicates all repositories in this PC will share the same configuration.
### Build a repository
#### Initialization
    $ git init
#### Add files to a repository
    $ git add <file>
    $ git commit -m "description"
You can use `git add` multiple times to track multiple files, and then use `git commit` to add all changes to the repository. The parameter `-m` indicates comments for this commit.

### Check Working directory
    $ git status
### Check changes
    $ git diff
    $ git diff --cached
    $ git diff HEAD -- <file>
- `git diff` check difference between _**work dir**_ and _**stage**_
- `git diff --cached` check difference between _**stage**_ and _**branch**_(current)
- `git diff HEAD -- <file>` check difference between _**work dir**_ and _**branch**_ (HEAD points at the latest commit of the current branch)
### Check log information
    $ git log --oneline --decorate --stat
- `--oneline` equals `--pretty=oneline --abbrev-commit`
- `--decorate` shows details about the branches and HEAD
- `--stat` shows brief information of insertion and deletion
### Check command log
    $ git reflog
### Version rollback
    $ git reset --soft/mixed/hard HEAD/HEAD^/HEAD^^/HEAD~100/commit_id/id_prefix
- `--soft` indicates no touching of _**work dic**_ or _**index(stage)**_ files, resetting **HEAD** to _<commit_id>_
- `--mixed`(default) indicates no touching of _**work dic**_ files, resetting _**index**_ and **HEAD** to _<commit_id>_
- `--hard` indicates resetting **ALL** things to _<commit_id>_ which means discarding all changes

`HEAD` points at the current version, `HEAD^` points at the previous version, `HEAD^^` points at the second previous version, `HEAD~100` points at the 100th previous version. We can also rollback to the certain version via commit id which can be obtained by `git log`.

### Discard changes(modification, removal)
#### Discard changes in work dir
    $ git checkout -- <file>
Rollback the file to the latest `git add` or `git commit`

#### Discard changes in stage
Step 1: Discard changes in _**stage**_
    $ git reset HEAD <file>
Step 2: Discard changes in _**work dir**_
    $ git checkout -- <file>
Tip:
    $ git reset --hard HEAD <file>
This is a combination of Step 1 and Step 2.

### Remove files
    $ git rm <file>
`git rm <file>` equals
    $ rm <file>
    $ git add <file>
### Remote repository
#### Generate SSH Key
    $ ssh-keygen -t rsa -C "youremail@example.com"
#### Add a remote repository
    $ git remote add origin https://github.com/username/repositoryname.git
    & git remote add origin git@github.com:username/repositoryname.git
#### Push to a remote repository
    $ git push -u origin master
`-u` indicates the first push of all files. `git push origin master` for the following push.

#### Clone from a remote repository
    $ git clone https://github.com/usern/repositoryname.git
Clone a whole project
    $ svn https://github.com/xx/xxx
Clone a directory. Note: replace 'tree' with 'branch' of the URL
    $ wget https://raw.github.com/xxx/xxx/xxx
Clone a single file. Note: use the URL of raw data

### Branch
#### Create a branch
    $ git branch <branchname>
#### Check branches
    $ git branch
`git branch` list all branches, attaching * at the current branch

#### Switch branches
    $ git checkout <branchname>

#### Create + Switch a branch
    $ git checkout -b <branchname>

#### Merge a branch to the current branch
    $ git merge <branchname>

#### Delete a branch
    $ git branch -d <branchname>

#### Check graph log about branches
    $ git log --graph

#### No-fast-forward merge
    $ git merge --no-ff -m "description" <branchname>
`--no-ff` indicates merging branches by generating another commit，in which case a comment is needed.

#### Save the current work
    $ git stash
#### Check saved work
    $ git stash list
#### Recover saved work
    $ git stash pop
#### Discard a branch which is never merged with parameter `-D`
    $ git branch -D <branchname>

#### Check the remote repository
    $ git remote -v

#### Create a local branch corresponding to the remote branch
    $ git checkout -b branch-name origin/branch-name
It is better for two branches to share a same name.

#### Add a connection between the local and remote branches
    $ git branch --set-upstream branch-name origin/branch-name
