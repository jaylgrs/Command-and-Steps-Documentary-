GIT & GITHUB CHEAT SHEET

A quick reference guide for common Git commands and workflows.

This guide covers cloning repositories, pushing changes, pulling updates, and uploading a local project to GitHub.

---

1. GIT CLONE — CLONE A REPOSITORY

---

Use this command to download an existing GitHub repository to your local machine.

Step 1: Open the repository on GitHub.

Step 2: Click Code > HTTPS > Copy URL.

Step 3: Open your terminal and navigate to the directory where you want to store the project.

cd C:\Users\Administrator\Downloads

Step 4: Clone the repository.

git clone https://github.com/USERNAME/REPOSITORY.git

Replace USERNAME and REPOSITORY with the appropriate GitHub username and repository name.

Step 5: Navigate into the project directory.

cd REPOSITORY

Step 6: Open the project in Visual Studio Code.

code .

The repository is now available on your local machine.

---

2. GIT PUSH — UPLOAD YOUR CHANGES

---

Use this workflow to upload your committed changes to GitHub.

Step 1: Check the current repository status.

git status

Step 2: Stage your changes.

git add .

This stages all eligible changes in the current repository, except files excluded by Git ignore rules.

Step 3: Create a commit.

git commit -m "Update project"

Replace the commit message with a short description of your changes.

Step 4: Push your commits to GitHub.

git push

If the local branch does not have an upstream branch configured, use:

git push -u origin main

Replace main if your branch has a different name.

---

3. GIT PULL — DOWNLOAD REMOTE UPDATES

---

Use this command to fetch and integrate changes from the configured remote branch.

git pull

Before pulling, check your repository status to identify any uncommitted local changes.

If Git reports conflicts, resolve them before continuing.

---

4. GIT STATUS — CHECK YOUR WORKING TREE

---

git status

This command shows the current state of your working tree and staging area, including modified files, untracked files, and changes ready to commit.

---

5. GIT LOG — VIEW COMMIT HISTORY

---

git log --oneline

Displays a compact list of previous commits and their messages.

---

6. UPLOAD AN EXISTING LOCAL PROJECT TO GITHUB

---

Use this workflow when you already have a local project and want to publish it to a new, empty GitHub repository.

Step 1: Navigate to your project directory.

cd PATH\TO\YOUR\PROJECT

Step 2: Initialize a Git repository.

git init

Step 3: Stage your files.

git add .

Step 4: Create the initial commit.

git commit -m "Initial commit"

Step 5: Rename the current branch to main.

git branch -M main

Step 6: Add your GitHub repository as a remote.

git remote add origin https://github.com/USERNAME/REPOSITORY.git

Step 7: Push your code to GitHub.

git push -u origin main

Replace the repository URL with your own GitHub repository URL.

Note: These instructions assume that the GitHub repository is empty. If it already contains commits, you may need to integrate the remote history first.

---

7. QUICK COMMAND REFERENCE

---

git clone URL
Clone a remote repository to your local machine.

git status
Check the working tree and staging area.

git add .
Stage eligible changes in the current directory.

git commit -m "message"
Create a commit containing staged changes.

git push
Upload local commits to the remote repository.

git pull
Fetch and integrate remote changes.

git log --oneline
View a compact commit history.

git branch
List local branches.

git remote -v
View configured remote repositories.

---

8. THE MOST COMMON GIT WORKFLOW

---

After modifying your code, use these commands to save your changes to GitHub.

Step 1: Check your changes.

git status

Step 2: Stage your changes.

git add .

Step 3: Commit your changes.

git commit -m "Describe your changes"

Step 4: Push your commits.

git push

---

IMPORTANT NOTES

- git add does not upload files to GitHub. It stages changes for the next commit.
- git commit saves a snapshot in your local Git history.
- git push uploads commits to the configured remote repository.
- git pull integrates remote changes into your current branch.
- If there are no staged changes, Git may report that there is nothing to commit.
- Never commit passwords, API tokens, private keys, or other secrets.
- Use a .gitignore file to exclude generated files, build outputs, and other files that should not be tracked.
- Check your repository status before pulling or switching branches.
- Avoid force-pushing unless you understand its effects on remote history.
- If a command fails, read the error message before trying additional commands.

For more information, visit the official Git documentation:

https://git-scm.com/doc

GitHub documentation:

https://docs.github.com/en/get-started

END OF GUIDE
