# Git and GitHub Tutorial Q&A

## 0:25 – What is Git and GitHub?
**Answer:**  
Git is a distributed version control system used to track changes in source code during software development.  
GitHub is a web-based platform that hosts Git repositories, allowing collaboration, version control, and code sharing among developers.

## 3:45 – Why are we using Git and GitHub?
**Answer:**  
Git and GitHub help in maintaining a history of code changes, collaborating with teams, reverting changes when needed, working on multiple features via branches, and contributing to open-source projects efficiently.

## 5:15 – Downloading Git  
**Answer:**  
Git can be downloaded from the official website: [https://git-scm.com](https://git-scm.com). Installers are available for Windows, macOS, and Linux.

## 6:00 – Structure of the Tutorial  
**Answer:**  
The tutorial follows a practical approach, starting from basic Git operations to advanced collaboration using GitHub, including branching, stashing, pull requests, and merge conflicts.

## 6:18 – Some Basic Linux Commands  
**Answer:**  
Commands like `ls`, `cd`, `pwd`, `mkdir`, `rm`, `touch`, `cat`, and `clear` are commonly used to navigate and manage files and directories in the terminal.

## 8:22 – Initializing a Git Repository  
**Answer:**  
Use `git init` inside a project folder to initialize a new Git repository.

## 9:48 – Making the First Change  
**Answer:**  
Create or modify a file in the repository using any editor or terminal command (e.g., `echo "Hello" > file.txt`).

## 13:43 – Staging the First Change  
**Answer:**  
Use `git add <filename>` to stage changes. To stage everything, use `git add .`.

## 14:40 – Committing the First Change  
**Answer:**  
Use `git commit -m "Commit message"` to commit staged changes.

## 16:12 – Adding Data to Files  
**Answer:**  
Use a text editor or commands like `echo "text" >> file.txt` to append data to existing files.

## 17:24 – Removing Changes from Stage  
**Answer:**  
Use `git reset <file>` to unstage a file. Use `git reset` to unstage everything.

## 18:14 – Viewing the Overall History of the Project  
**Answer:**  
Use `git log` to see the commit history of the repository.

## 18:52 – Making Few More Commits  
**Answer:**  
Repeat the process: make changes, stage them using `git add`, and commit with `git commit -m "message"`.

## 19:31 – Removing a Commit from the History  
**Answer:**  
Use `git reset --hard <commit_id>` to remove recent commits.  
Caution: This permanently deletes commits from history.

## 21:35 – Stashing Changes  
**Answer:**  
Use `git stash` to temporarily save changes without committing them.

## 24:25 – Popping Stash  
**Answer:**  
Use `git stash pop` to apply the most recent stash and remove it from the stash list.

## 25:00 – Clearing Stash  
**Answer:**  
Use `git stash clear` to delete all saved stashes.

## 25:26 – Starting GitHub  
**Answer:**  
Create an account on [GitHub](https://github.com), install Git, and set up SSH or HTTPS for authentication.

## 26:10 – Creating a New Repository on GitHub  
**Answer:**  
Click the “New” button on your GitHub profile or organization and follow the steps to create a new repository.

## 26:40 – Connecting Remote Repository  
**Answer:**  
Use `git remote add origin <repo_url>` to connect local Git to GitHub.  
Use `git remote -v` to view remotes.

## 28:05 – Pushing Local Changes to Remote Repository  
**Answer:**  
Use `git push -u origin main` (or `master`) to push changes for the first time.

## 28:43 – What Are Branches?  
**Answer:**  
Branches allow you to develop features or fix bugs independently from the main codebase.

## 31:10 – Use of Branches  
**Answer:**  
They help manage different lines of development and avoid breaking the main code. Multiple developers can work simultaneously.

## 32:42 – Making a New Branch and Switching to It  
**Answer:**  
Use `git checkout -b <branch-name>` to create and switch to a new branch.

## 34:28 – Merging Branch to Main  
**Answer:**  
Switch to `main` using `git checkout main`, then use `git merge <branch-name>` to merge.

## 36:00 – Pushing New Changes to Master Branch  
**Answer:**  
After merging, use `git push origin main` to update the remote repository.

## 37:10 – Working with Existing Projects  
**Answer:**  
Clone the repository using `git clone <repo_url>`. Make changes and push/pull as needed.

## 37:38 – Why Fork and How to Fork?  
**Answer:**  
Forking creates a personal copy of a repository. Click the “Fork” button on GitHub to fork any public repo.

## 39:00 – Cloning the Forked Project  
**Answer:**  
Use `git clone <your_fork_url>` to clone the forked repo to your local machine.

## 40:10 – What is Upstream and Adding It  
**Answer:**  
"Upstream" refers to the original repo. Add it via:  
`git remote add upstream <original_repo_url>`

## 41:00 – What is a Pull Request?  
**Answer:**  
A pull request lets you submit code changes from your fork/branch to the original repo for review and merging.

## 45:28 – Never Commit on Main Branch & First Pull Request  
**Answer:**  
Work on branches, not `main`, to keep history clean. Push the branch and open a pull request from it.

## 51:04 – Removing a Commit from Pull Request  
**Answer:**  
Use `git reset --hard <commit_id>` to remove unwanted commits, then `git push -f` to force update the pull request.

## 52:56 – Merging a Pull Request  
**Answer:**  
Repo owners can review and merge PRs via GitHub's interface using Merge, Squash, or Rebase options.

## 53:28 – Making Forked Project Even with Main Project  
**Answer:**  
Fetch upstream changes and merge them:  
```bash
git fetch upstream  
git checkout main  
git merge upstream/main  
git push origin main
```

## 59:46 – Instructions to Practice  
**Answer:**  
Try creating your own repository, practice commands like clone, add, commit, push, pull, stash, and create pull requests.

## 1:00:20 – Squashing Commits  
**Answer:**  
Use `git rebase -i HEAD~n` (where n = number of commits to squash), then mark the commits as `squash` or `fixup`.

## 1:01:59 – Using the Rebase Command  
**Answer:**  
`git rebase <branch>` rewrites commits from one branch onto another, creating a linear history.

## 1:04:35 – Using the Hard Flag to Reset  
**Answer:**  
`git reset --hard <commit_id>` resets the working directory and index to a specific commit (irreversible).

## 1:05:11 – Merge Conflicts and How to Resolve Them  
**Answer:**  
When two changes clash, Git marks them in files. Open the file, edit manually to fix, then stage (`git add`) and commit.
