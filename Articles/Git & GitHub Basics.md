# Git & GitHub Basics

Created: September 24, 2024 5:17 PM

### Setup on a Local Machine

```bash
git init  # initiate a git project
git status  # check local environment
git add <filename>  # staging a file

git config --global user.email "chriswang2025@u.northwestern.edu"
git config --global user.name "Chris Wang"

git commit -m "Create the initial message"
# insertions mean lines
git log  # to see who and when committed (see log)
```

### GitHub Setup

- Create a new repository
- Get the repository’s URL

```bash
git remote add origin https://github.com/BellowAverage/HelloWorld.git
git branch -M main  # rename the current branch (master) to main
git push -u origin main  # origin is the name of a repository
```

### Commonly Used Commands

```bash
git branch  # see current branch
git branch -r  # list all branches available
git checkout develop  # switch to another branch named develop, for example
git checkout -b <branch_name>  # Creates and switches to a new branch
git merge <branch_name>  # Merges the specified branch into the current branch.
git push -u origin <branch_name>  # Pushes the current branch to the remote repository and sets the upstream branch.
git pull  # Fetches and merges changes from the remote repository into the current branch
# -------------------------
# 3 most commonly used git command recommended by professor Thomas Miller
git status  # to see where I'm at
git add .  # take care of all the files
git commit -m "<message>"
```

### Concluded by ChatGPT from Atlassian Knowledge Base

| Command | Description | Example |
| --- | --- | --- |
| `git init` | Initializes a new Git repository | `git init` |
| `git clone <repository_url>` | Clones a repository to your local machine | `git clone https://github.com/user/repo.git` |
| `git config --global user.name` | Sets your Git username globally | `git config --global user.name "Your Name"` |
| `git config --global user.email` | Sets your Git email globally | `git config --global user.email "your.email@example.com"` |
| `git status` | Displays the status of your working directory | `git status` |
| `git add <file>` | Adds a file to the staging area | `git add myfile.txt` |
| `git add .` | Adds all changes in the current directory | `git add .` |
| `git commit -m "message"` | Commits changes with a message | `git commit -m "Initial commit"` |
| `git log` | Shows the commit history | `git log` |
| `git log --oneline` | Shows a compact commit history | `git log --oneline` |
| `git checkout -b <branch>` | Creates and switches to a new branch | `git checkout -b feature-branch` |
| `git checkout <branch>` | Switches to an existing branch | `git checkout main` |
| `git merge <branch>` | Merges a branch into the current one | `git merge feature-branch` |
| `git push -u origin <branch>` | Pushes changes to the remote repository | `git push -u origin main` |
| `git pull` | Fetches and merges changes from the remote | `git pull` |
| `git stash` | Temporarily saves uncommitted changes | `git stash` |
| `git stash apply` | Applies stashed changes | `git stash apply` |
| `git remote -v` | Lists remote repositories | `git remote -v` |
| `git remote add <name> <url>` | Adds a remote repository | `git remote add origin https://github.com/user/repo.git` |
| `git remote rm <name>` | Removes a remote repository | `git remote rm origin` |
| `git fetch <remote>` | Fetches updates from the remote | `git fetch origin` |
| `git rebase <branch>` | Reapplies commits on top of another branch | `git rebase main` |
| `git reset --soft <commit>` | Resets HEAD to a previous commit, keeping changes staged | `git reset --soft HEAD~1` |
| `git reset --mixed <commit>` | Resets HEAD and unstages changes | `git reset --mixed HEAD~1` |
| `git reset --hard <commit>` | Resets the working directory and index | `git reset --hard HEAD~1` |
| `git revert <commit>` | Reverts a specific commit | `git revert <commit_hash>` |
| `git branch -d <branch>` | Deletes a branch | `git branch -d feature-branch` |
| `git show <commit>` | Displays information about a commit | `git show <commit_hash>` |
| `git stash pop` | Applies and removes the latest stash | `git stash pop` |
| `git log --graph` | Shows a graph of commits | `git log --graph` |
| `git log --decorate` | Shows commit history with references | `git log --decorate` |
| `git tag <tag>` | Creates a lightweight tag | `git tag v1.0.0` |
| `git tag -a <tag> -m "message"` | Creates an annotated tag | `git tag -a v1.0.0 -m "Version 1.0"` |
| `git push origin --tags` | Pushes tags to the remote repository | `git push origin --tags` |
| `git rebase -i HEAD~n` | Interactively rebases the last `n` commits | `git rebase -i HEAD~3` |
| `git cherry-pick <commit>` | Applies a specific commit to the current branch | `git cherry-pick <commit_hash>` |
| `git checkout <commit>` | Switches to a specific commit | `git checkout <commit_hash>` |
| `git diff <commit1> <commit2>` | Shows differences between two commits | `git diff <commit1> <commit2>` |
| `git blame <file>` | Shows who last modified each line of a file | `git blame myfile.txt` |
| `git bisect` | Helps find the commit that introduced a bug | `git bisect` |
| `git reflog` | Shows a log of all reference updates | `git reflog` |
| `git cat-file -p <commit>` | Shows the content of a commit object | `git cat-file -p <commit_hash>` |
| `git rev-parse <ref>` | Retrieves the SHA-1 hash of a reference | `git rev-parse HEAD` |
| `git fsck` | Verifies the integrity of the repository | `git fsck` |
| `git gc` | Cleans up unnecessary files in the repository | `git gc` |
| `git submodule add <repo> <path>` | Adds a submodule to the repository | `git submodule add https://github.com/user/submodule.git` |
| `git submodule init` | Initializes submodules | `git submodule init` |
| `git submodule update` | Updates submodules | `git submodule update` |
| `git grep <pattern>` | Searches for a pattern in tracked files | `git grep "TODO"` |