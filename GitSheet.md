Git & GitHub Cheat Sheet

A quick reference guide for common Git commands and workflows.

This guide covers cloning repositories, pushing changes, pulling updates, and uploading local projects to GitHub.

---

Table of Contents

1. "Git Clone — Clone a Repository" (#1-git-clone--clone-a-repository)
2. "Git Push — Upload Your Changes" (#2-git-push--upload-your-changes)
3. "Git Pull — Download Remote Updates" (#3-git-pull--download-remote-updates)
4. "Git Status — Check Your Working Tree" (#4-git-status--check-your-working-tree)
5. "Git Log — View Commit History" (#5-git-log--view-commit-history)
6. "Upload a Local Project to GitHub" (#6-upload-a-local-project-to-github)
7. "Quick Command Reference" (#7-quick-command-reference)
8. "Common Git Workflow" (#8-common-git-workflow)
9. "Important Notes" (#9-important-notes)

---

1. Git Clone — Clone a Repository

Use "git clone" to download an existing GitHub repository to your local machine.

Step 1: Copy the repository URL

Open your repository on GitHub, click Code → HTTPS, and copy the URL.

Step 2: Navigate to your preferred directory

Open your terminal and navigate to the directory where you want to store the project.

cd C:\Users\Administrator\Downloads

Step 3: Clone the repository

Replace "USERNAME" and "REPOSITORY" with your GitHub username and repository name.

git clone https://github.com/USERNAME/REPOSITORY.git

Step 4: Enter the project directory

cd REPOSITORY

Replace "REPOSITORY" with the actual folder name created by Git.

Step 5: Open the project in Visual Studio Code

code .

Your repository is now available locally and ready for development.

---

2. Git Push — Upload Your Changes

Use this workflow to upload your committed changes to GitHub.

Step 1: Check the repository status

git status

This shows which files have been modified, added, or staged.

Step 2: Stage your changes

git add .

This stages eligible changes in the current directory and its subdirectories, except files excluded by Git ignore rules.

Step 3: Create a commit

git commit -m "Update project"

Replace "Update project" with a short description of your changes.

Step 4: Push your commits to GitHub

git push

If your local branch does not have an upstream branch configured, use:

git push -u origin main

Replace "main" with your actual branch name if necessary. The "-u" option configures the upstream branch for subsequent pushes.

---

3. Git Pull — Download Remote Updates

Use "git pull" to fetch and integrate changes from the configured remote branch.

git pull

Before pulling, check your repository status to identify any uncommitted local changes.

If Git reports merge conflicts, resolve them before continuing.

---

4. Git Status — Check Your Working Tree

git status

This command shows the current state of your working tree and staging area, including modified files, untracked files, and changes ready to commit.

---

5. Git Log — View Commit History

git log --oneline

Displays a compact list of previous commits and their messages.

---

6. Upload a Local Project to GitHub

Use this workflow when you already have a local project and want to publish it to a new, empty GitHub repository.

Step 1: Navigate to your project directory

cd PATH/TO/YOUR/PROJECT

Replace the path with your actual project directory.

Step 2: Initialize Git

git init

Step 3: Stage your files

git add .

Step 4: Create the initial commit

git commit -m "Initial commit"

Step 5: Rename the current branch to "main"

git branch -M main

Step 6: Connect your GitHub repository

git remote add origin https://github.com/USERNAME/REPOSITORY.git

Replace the URL with your actual GitHub repository URL.

Step 7: Push your project

git push -u origin main

Your local project is now uploaded to GitHub.

Note: These steps assume that the GitHub repository is empty. If it already contains commits, you may need to integrate the remote history before pushing.

---

7. Quick Command Reference

Command| Purpose
"git clone URL"| Clone a remote repository
"git status"| Check the working tree and staging area
"git add ."| Stage eligible changes
"git commit -m "message""| Create a commit
"git push"| Upload local commits
"git pull"| Fetch and integrate remote changes
"git log --oneline"| View commit history
"git branch"| List local branches
"git remote -v"| View configured remote repositories
"git diff"| View unstaged changes

---

8. Common Git Workflow

After modifying your code, follow these steps to save and upload your changes to GitHub.

Step 1: Check your changes

git status

Step 2: Stage your changes

git add .

Step 3: Commit your changes

git commit -m "Describe your changes"

Step 4: Push your commits

git push

Quick tip: You can reuse this workflow whenever you finish a feature, fix a bug, or update your project.

---

9. Important Notes

- "git add" stages changes; it does not upload them to GitHub.
- "git commit" saves a snapshot in your local Git history.
- "git push" uploads commits to the configured remote repository.
- "git pull" fetches and integrates remote changes into your current branch.
- If there are no staged changes, Git may report that there is nothing to commit.
- Never commit passwords, API tokens, private keys, or other secrets.
- Use a ".gitignore" file to exclude generated files, build outputs, and other files that should not be tracked.
- Check your repository status before pulling or switching branches.
- Avoid force-pushing unless you understand its effects on remote history.
- If a command fails, read the error message before trying additional commands.

---

Official Documentation

- "Git Documentation" (https://git-scm.com/doc)
- "GitHub Documentation" (https://docs.github.com/en/get-started)
- "GitHub: About Remote Repositories" (https://docs.github.com/en/get-started/git-basics/about-remote-repositories)

---

Created as a practical reference for developers learning and using Git and GitHub.

End of Guide
