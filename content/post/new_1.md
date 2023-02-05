---
title: "Git Cheatsheet"
date: 2022-10-14T13:23:31+05:30
#draft: true
---

# CREATE REPOSITORIES

```shell
New repostiories can be created either locally or by copying a file that alredy exists on GitHub.  
**$ git init** - Turn an existing directory into git repository.  
**$ git clone [url]** - Clone a repository that already exists in GitHub including all the files, branches and commits.
```

## CONFIGURE  
Configure user information for all local repositories.  
**$ git config --global user.name "[name]"** - Sets the name you want attached to your commit transactions.  
**$ git config --global user.email "[email-address]"** - Sets the email you want attached to your commit transactions.  
**$ git config --global color.ui auto** - Enables helpuful colorization of command line output.  
**BRANCHES**  
All the commits will be made to the branches you are currently checked out to.  
**$ git branch [branch-name]** - Will create a new branch.  
**$ git checkout [branch-name]** - Switches to the specified branch and updates the working directory.  
**$ git checkout [branch-name] -b** - Creates a new branch and switches to the specified branch and updates the working directory.  
**$ git merge [branch-name]** - Combines the specified branch’s history into the
current branch. This is usually done in pull requests.  
**$ git branch -d [branch-name]** - Deletes the specified branch.  
**SYNCHRONIZE CHANGES**  
Synchronize your local repository with the remote repository on GitHub.  
**$ git push** - Uploads all local branch commits to GitHub.  
**$ git merge** - Combines remote tracking branch into current local branch.  
**$ git fetch** - Downloads all history from the remote tracking branches.  
**$ git pull** - Updates your current local working branch with all new commits from the corresponding remote branch on GitHub. git pull is a combination of git fetch and git merge.  
**MAKE CHANGES**  
Browse and inspect the evolution of project files.  
**$ git diff [first-branch]...[second-branch]** - Shows content differences between two branches.  
**$ git log --follow [file]** - Lists version history for a file, including renames.  
**$ git log** - Lists version history for the current branch.  
**REDO COMMITS**  
Erase mistakes and craft replacement history.  
**$ git reset [commit]** - Undoes all commits after [commit], preserving changes locally.  
**$ git reset --hard [commit]** - Discards all history and changes back to the specified commit.  