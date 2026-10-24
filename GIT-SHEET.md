# Git & GitHub Cheat Sheet

A practical reference guide for common Git commands and GitHub workflows.

This guide covers repository setup, cloning, staging, committing, pushing, pulling, authentication, branch management, merge conflicts, remote synchronization, and troubleshooting.

---

## Table of Contents

1. [Git Installation and Configuration](#1-git-installation-and-configuration)
2. [Git Clone — Clone a Repository](#2-git-clone--clone-a-repository)
3. [Git Init — Initialize a Repository](#3-git-init--initialize-a-repository)
4. [Git Status — Check Your Working Tree](#4-git-status--check-your-working-tree)
5. [Git Add — Stage Changes](#5-git-add--stage-changes)
6. [Git Commit — Save Changes](#6-git-commit--save-changes)
7. [Git Push — Upload Your Changes](#7-git-push--upload-your-changes)
8. [Git Pull — Download Remote Updates](#8-git-pull--download-remote-updates)
9. [Git Fetch — Check Remote Updates](#9-git-fetch--check-remote-updates)
10. [Git Log — View Commit History](#10-git-log--view-commit-history)
11. [Git Branch — Manage Branches](#11-git-branch--manage-branches)
12. [Git Remote — Manage Remote Repositories](#12-git-remote--manage-remote-repositories)
13. [Upload a Local Project to GitHub](#13-upload-a-local-project-to-github)
14. [Connect an Existing Local Project to GitHub](#14-connect-an-existing-local-project-to-github)
15. [Fix Rejected Pushes](#15-fix-rejected-pushes)
16. [Resolve Merge Conflicts](#16-resolve-merge-conflicts)
17. [Fix Unrelated Git Histories](#17-fix-unrelated-git-histories)
18. [Git Diff — Review Changes](#18-git-diff--review-changes)
19. [Git Restore — Discard Changes](#19-git-restore--discard-changes)
20. [Git Stash — Temporarily Save Changes](#20-git-stash--temporarily-save-changes)
21. [Undo Commits Safely](#21-undo-commits-safely)
22. [Authentication and GitHub Access](#22-authentication-and-github-access)
23. [Common Git Workflow](#23-common-git-workflow)
24. [Troubleshooting Guide](#24-troubleshooting-guide)
25. [Quick Command Reference](#25-quick-command-reference)
26. [Important Notes](#26-important-notes)
27. [Official Documentation](#27-official-documentation)

---

## 1. Git Installation and Configuration

### Check Whether Git Is Installed

```powershell
git --version
```

If Git is installed, the command displays the installed version.

### Configure Your Git Identity

Set the name and email associated with your commits.

```powershell
git config --global user.name "Jay"
git config --global user.email "YOUR_EMAIL@example.com"
```

Replace the example email with the email address you want associated with your commits.

These settings identify the author of commits. They do not authenticate you to GitHub.

### Verify Your Configuration

```powershell
git config --global --list
```

Check your configured name and email.

---

## 2. Git Clone — Clone a Repository

Use `git clone` to download an existing repository to your computer.

### Step 1: Navigate to Your Preferred Directory

```powershell
cd C:\Users\Administrator\Downloads
```

### Step 2: Clone the Repository

```powershell
git clone https://github.com/USERNAME/REPOSITORY.git
```

Replace `USERNAME` and `REPOSITORY` with the appropriate values.

### Step 3: Enter the Project Directory

```powershell
cd REPOSITORY
```

### Step 4: Open the Project in Visual Studio Code

```powershell
code .
```

### Step 5: Verify the Repository

```powershell
git status
```

**Important:** When you clone a repository, Git automatically initializes the repository and configures its remote connection. You do not need to run `git init` afterward.

---

## 3. Git Init — Initialize a Repository

Use `git init` when you already have a local project that is not yet a Git repository.

### Step 1: Navigate to Your Project

```powershell
cd PATH\TO\YOUR\PROJECT
```

### Step 2: Initialize Git

```powershell
git init
```

This creates a `.git` directory containing the repository's local Git data.

### Step 3: Verify the Repository

```powershell
git status
```

If the repository is initialized successfully, Git displays its current status.

**Important:** Run `git init` only when you need to initialize a repository. You do not need to run it every time you modify your code.

---

## 4. Git Status — Check Your Working Tree

```powershell
git status
```

This displays the state of your working tree and staging area.

Common statuses include:

| Status | Meaning |
|---|---|
| `Untracked files` | Files Git is not tracking yet |
| `Changes not staged for commit` | Tracked files have modifications that are not staged |
| `Changes to be committed` | Changes are staged and ready to commit |
| `nothing to commit, working tree clean` | No uncommitted changes are present |
| `You have unmerged paths` | A merge conflict must be resolved |
| `All conflicts fixed but you are still merging` | Conflicts are resolved, but the merge still needs to be concluded |

**Tip:** When a Git command fails, check `git status` before running additional commands.

---

## 5. Git Add — Stage Changes

### Stage All Eligible Changes

```powershell
git add .
```

Stages eligible changes under the current directory, subject to Git ignore rules.

### Stage a Specific File

```powershell
git add README.md
```

### Stage All Changes Across the Repository

```powershell
git add -A
```

This stages additions, modifications, and deletions throughout the repository.

**Important:** `git add` does not upload files to GitHub. It prepares changes for the next commit.

---

## 6. Git Commit — Save Changes

A commit saves a snapshot of the staged changes in your local Git history.

### Create a Commit

```powershell
git commit -m "Update project"
```

Use a short message that describes the changes.

Examples:

```powershell
git commit -m "Add entity distance configuration"
git commit -m "Fix entity rendering logic"
git commit -m "Update README documentation"
```

### Check Whether a Commit Is Needed

```powershell
git status
```

If Git reports:

```text
nothing to commit, working tree clean
```

There are no uncommitted changes to save.

**Important:** A successful commit does not automatically upload your changes to GitHub. You still need to push your commits.

---

## 7. Git Push — Upload Your Changes

Use `git push` to upload local commits to a configured remote repository.

### Push to an Existing Upstream Branch

```powershell
git push
```

### Push a Branch for the First Time

```powershell
git push -u origin main
```

The `-u` option sets the upstream branch so subsequent pushes can usually use `git push`.

Replace `main` if your branch has a different name.

### If Git Reports `fetch first`

Example:

```text
! [rejected] main -> main (fetch first)
error: failed to push some refs
```

The remote branch contains commits that are not incorporated into your local branch.

Do not immediately force-push.

First, inspect the repository:

```powershell
git status
git fetch origin
git log --oneline --graph --decorate --all -10
```

Then determine whether you need to integrate remote commits, resolve conflicts, or address another repository-history problem.

See [Section 15: Fix Rejected Pushes](#15-fix-rejected-pushes).

---

## 8. Git Pull — Download Remote Updates

`git pull` fetches remote changes and integrates them into your current branch.

### Pull from the Configured Upstream

```powershell
git pull
```

### Pull from a Specific Remote Branch

```powershell
git pull origin main
```

### If the Local and Remote Histories Are Unrelated

If the local repository and GitHub repository were initialized independently, Git may reject the pull with:

```text
fatal: refusing to merge unrelated histories
```

You can explicitly allow the histories to be merged:

```powershell
git pull origin main --allow-unrelated-histories
```

Use this only when you intend to combine the two histories.

If conflicts occur, resolve them before continuing.

**Important:** Check `git status` after a failed pull. Git may have started a merge that still needs to be completed or aborted.

---

## 9. Git Fetch — Check Remote Updates

Use `git fetch` to retrieve information about remote commits without integrating those changes into your current branch.

### Fetch from the Default Remote

```powershell
git fetch origin
```

### Compare Local and Remote Commits

```powershell
git log --oneline --graph --decorate --all -10
```

Example:

```text
* abc1234 (HEAD -> main) Update local project
* def5678 (origin/main) Previous remote commit
```

This output helps you identify which commits belong to your local branch and which belong to the remote-tracking branch.

### Check the Difference Between Local and Remote

After fetching:

```powershell
git log --oneline main..origin/main
```

Shows commits on `origin/main` that are not reachable from local `main`.

```powershell
git log --oneline origin/main..main
```

Shows commits on local `main` that are not reachable from `origin/main`.

**Important:** These commands help diagnose divergence. They do not merge or modify your branches.

---

## 10. Git Log — View Commit History

### View Compact History

```powershell
git log --oneline
```

### View Branches and Merge History

```powershell
git log --oneline --graph --decorate --all -10
```

### View the Latest Commit

```powershell
git log -1
```

Use these commands to inspect commit history before attempting complicated recovery operations.

---

## 11. Git Branch — Manage Branches

### List Local Branches

```powershell
git branch
```

### List Local and Remote-Tracking Branches

```powershell
git branch -a
```

### Rename the Current Branch to `main`

```powershell
git branch -M main
```

### Create a New Branch

```powershell
git branch feature-name
```

### Switch to a Branch

```powershell
git switch feature-name
```

### Create and Switch to a New Branch

```powershell
git switch -c feature-name
```

**Tip:** Use branches to work on features separately from your main development branch.

---

## 12. Git Remote — Manage Remote Repositories

### View Configured Remotes

```powershell
git remote -v
```

### Add a Remote

```powershell
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

### Change the Remote URL

```powershell
git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
```

### Remove a Remote

```powershell
git remote remove origin
```

Removing a remote only removes the local remote configuration. It does not delete the GitHub repository.

### Verify the Remote

```powershell
git remote -v
```

**Important:** If `origin` already exists, running `git remote add origin ...` again will fail. Check the existing remote or update it with `git remote set-url`.

---

## 13. Upload a Local Project to GitHub

Use this workflow when you have a local project and want to publish it to a new, empty GitHub repository.

### Step 1: Navigate to the Project

```powershell
cd PATH\TO\YOUR\PROJECT
```

### Step 2: Initialize Git

```powershell
git init
```

### Step 3: Review the Files

```powershell
git status
```

Make sure you have an appropriate `.gitignore` file before staging generated files, build outputs, or other files that should not be tracked.

### Step 4: Stage the Files

```powershell
git add .
```

### Step 5: Create the Initial Commit

```powershell
git commit -m "Initial commit"
```

### Step 6: Rename the Branch

```powershell
git branch -M main
```

### Step 7: Add the GitHub Remote

```powershell
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

### Step 8: Push the Project

```powershell
git push -u origin main
```

Your local project should now be uploaded to GitHub.

**Important:** These steps assume the GitHub repository is empty. If it already contains a README, license, or other commits, you may need to integrate the remote history before pushing.

See [Section 15: Fix Rejected Pushes](#15-fix-rejected-pushes).

---

## 14. Connect an Existing Local Project to GitHub

Use this workflow when your local project already has Git history and you want to connect it to a GitHub repository.

### Step 1: Check the Current Repository

```powershell
git status
git remote -v
```

### Step 2: Add the Remote if Necessary

```powershell
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

Only run this if `origin` does not already exist.

### Step 3: Inspect the Remote History

```powershell
git fetch origin
git log --oneline --graph --decorate --all -10
```

### Step 4: Push if the Histories Are Compatible

```powershell
git push -u origin main
```

If Git rejects the push, do not immediately force-push. Inspect the error and follow the appropriate troubleshooting steps.

---

## 15. Fix Rejected Pushes

A rejected push does not necessarily mean your local files are lost.

The correct fix depends on the error message.

### Case A: `fetch first`

Example:

```text
! [rejected] main -> main (fetch first)
```

The remote branch contains commits that your local branch does not incorporate.

#### Step 1: Check Your Working Tree

```powershell
git status
```

Resolve or save uncommitted work before continuing.

#### Step 2: Fetch Remote Updates

```powershell
git fetch origin
```

#### Step 3: Inspect the History

```powershell
git log --oneline --graph --decorate --all -10
```

#### Step 4: Integrate the Remote History

If the histories are already related and a normal merge is appropriate:

```powershell
git merge origin/main
```

If Git reports unrelated histories and you intentionally need to combine them:

```powershell
git merge origin/main --allow-unrelated-histories
```

Resolve any conflicts and complete the merge.

#### Step 5: Push Again

```powershell
git push -u origin main
```

If the merge succeeds and no new remote commits appear, the push should now be able to include the integrated history.

### Case B: `non-fast-forward`

Example:

```text
! [rejected] main -> main (non-fast-forward)
```

The remote branch has history that cannot be advanced directly from your local branch.

Fetch the latest changes, inspect the history, and integrate the remote commits before pushing.

```powershell
git fetch origin
git log --oneline --graph --decorate --all -10
```

Do not assume every rejected push has the same cause.

### Case C: `src refspec main does not match any`

Possible causes include:
- The local branch is not named `main`.
- No commit exists yet.

Check the current branch:

```powershell
git branch
```

Check the repository status:

```powershell
git status
```

If the project has no commits yet, stage the intended files and create an initial commit.

### Case D: `remote origin already exists`

Check the current remote:

```powershell
git remote -v
```

If the URL is incorrect, update it:

```powershell
git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
```

### Case E: Push Still Fails After a Merge

Check:

```powershell
git status
git fetch origin
git log --oneline --graph --decorate --all -10
```

Look for:
- A merge that has not been committed.
- New commits on `origin/main`.
- Unrelated histories that have not been integrated.
- A branch name mismatch.

Do not repeatedly run `git push` without investigating the error.

---

## 16. Resolve Merge Conflicts

A merge conflict occurs when Git cannot automatically reconcile changes between branches.

### Step 1: Check the Status

```powershell
git status
```

Git identifies files that need conflict resolution.

### Step 2: Open the Conflicted File

For example:

```powershell
code README.md
```

### Step 3: Understand the Conflict Markers

```text
<<<<<<< HEAD
Local version
=======
Incoming version
>>>>>>> branch-name
```

The markers separate competing versions of the content.

- `<<<<<<< HEAD`: The current branch's version.
- `=======`: Separates the two versions.
- `>>>>>>> branch-name`: The incoming version.

The exact labels can vary depending on the operation.

### Step 4: Resolve the File

Edit the file and choose which content to keep, or combine both versions.

Remove all conflict markers and save the file.

### Step 5: Stage the Resolved File

```powershell
git add README.md
```

Replace `README.md` with the relevant filename.

### Step 6: Check the Status Again

```powershell
git status
```

If Git reports:

```text
All conflicts fixed but you are still merging.
(use "git commit" to conclude merge)
```

Conclude the merge:

```powershell
git commit -m "Resolve merge conflicts"
```

If Git has already created a merge commit automatically, do not create an unnecessary additional commit.

### Step 7: Push the Completed Merge

```powershell
git push
```

**Important:** Staging a resolved file does not always finish the merge. Follow the instructions shown by `git status`.

### Abort an Unwanted Merge

If you want to cancel a merge that is still in progress:

```powershell
git merge --abort
```

Use this only when you intend to abandon the current merge.

Check your status afterward.

---

## 17. Fix Unrelated Git Histories

This can happen when you independently initialize a local repository and a GitHub repository.

For example:
- The local project has its own initial commit.
- The GitHub repository has a separate initial commit created with a README.

Git may report:

```text
fatal: refusing to merge unrelated histories
```

### Step 1: Check Your Working Tree

```powershell
git status
```

### Step 2: Fetch the Remote History

```powershell
git fetch origin
```

### Step 3: Inspect the Commit Graph

```powershell
git log --oneline --graph --decorate --all -10
```

### Step 4: Merge the Histories

If you intend to preserve and combine both histories:

```powershell
git merge origin/main --allow-unrelated-histories
```

### Step 5: Resolve Conflicts

If files conflict, resolve them as described in Section 16.

### Step 6: Complete the Merge

```powershell
git status
```

If Git says the merge is still in progress:

```powershell
git add .
git commit -m "Merge remote history"
```

Only run the commit if a merge commit is still required.

### Step 7: Push

```powershell
git push -u origin main
```

**Important:** Do not use `--allow-unrelated-histories` routinely. Use it only when you intentionally need to combine independently created histories.

---

## 18. Git Diff — Review Changes

### Review Unstaged Changes

```powershell
git diff
```

### Review Staged Changes

```powershell
git diff --staged
```

### Compare Local and Remote Branches

After fetching remote changes:

```powershell
git diff main origin/main
```

This compares the content of the two branches.

Use `git log` as well if you need to understand the commit history.

---

## 19. Git Restore — Discard Changes

Use `git restore` carefully. Some operations permanently discard uncommitted work.

### Discard Unstaged Changes in a File

```powershell
git restore README.md
```

This restores the file to its index version, discarding its unstaged changes.

### Unstage a File Without Discarding Its Changes

```powershell
git restore --staged README.md
```

This removes the file from the staging area while preserving its working-tree changes.

### Important Warning

Do not use `git restore` to discard a file unless you understand which changes will be lost.

If the changes are important, save a backup or commit them first.

---

## 20. Git Stash — Temporarily Save Changes

Use `git stash` when you need to temporarily set aside uncommitted changes.

### Save Tracked Changes

```powershell
git stash push -m "Temporary changes"
```

### Include Untracked Files

```powershell
git stash push -u -m "Temporary changes"
```

### List Saved Stashes

```powershell
git stash list
```

### Restore the Latest Stash and Keep It Saved

```powershell
git stash apply
```

### Restore the Latest Stash and Remove It from the Stash List

```powershell
git stash pop
```

**Important:** Stashing is not a replacement for a permanent backup. Verify that your changes were restored correctly.

---

## 21. Undo Commits Safely

### Create a Reverting Commit

```powershell
git revert COMMIT_ID
```

Replace `COMMIT_ID` with the commit hash you want to reverse.

This creates a new commit that reverses the changes introduced by the specified commit.

This is generally safer for commits that have already been pushed and shared.

### Inspect Recent Commits

```powershell
git log --oneline -5
```

Review the history before choosing an undo operation.

**Warning:** Commands such as `git reset --hard` can discard local changes and move your branch backward. Avoid using them unless you understand the consequences and have preserved any important work.

---

## 22. Authentication and GitHub Access

GitHub may open a browser when you push over HTTPS:

```text
info: please complete authentication in your browser...
```

Complete the authentication flow in your browser.

This message alone is not an error.

### Authentication Is Different from Git Identity

These settings:

```powershell
git config --global user.name "Jay"
git config --global user.email "YOUR_EMAIL@example.com"
```

configure the author information used in commits.

They do not log you in to GitHub.

### If Authentication Fails

Check:
- Whether you completed the browser authentication.
- Whether you are signed in to the correct GitHub account.
- Whether the account has permission to push to the repository.
- Whether the remote URL points to the intended repository.

Verify the remote:

```powershell
git remote -v
```

For more help, consult the official [GitHub authentication documentation](https://docs.github.com/en/authentication).

**Security note:** Never share passwords, access tokens, private keys, or other credentials in your repository or chat.

---

## 23. Common Git Workflow

Use this workflow after modifying code in an existing repository.

### Step 1: Check the Status

```powershell
git status
```

### Step 2: Review the Changes

```powershell
git diff
```

### Step 3: Stage the Changes

```powershell
git add .
```

### Step 4: Create a Commit

```powershell
git commit -m "Describe your changes"
```

### Step 5: Push the Commit

```powershell
git push
```

### Optional: Get Remote Updates First

If you need to integrate remote changes before pushing:

```powershell
git fetch origin
git status
```

Inspect the history and integrate changes as needed.

**Tip:** A good commit message describes the actual change, such as `Fix entity distance calculation` rather than `Update`.

---

## 24. Troubleshooting Guide

| Error or Message | Meaning | Recommended Action |
|---|---|---|
| `fatal: not a git repository` | The current directory is not inside a Git repository | Navigate to the project directory or initialize Git with `git init` |
| `nothing to commit, working tree clean` | No uncommitted changes exist | Check the history or make a new change if needed |
| `Changes not staged for commit` | Changes have not been staged | Use `git add <file>` |
| `untracked files` | Files are not tracked by Git | Review them, then use `git add` if appropriate |
| `fetch first` | Remote history contains commits not incorporated locally | Fetch and inspect the commit graph |
| `non-fast-forward` | The remote branch cannot be advanced directly from the local branch | Integrate remote changes before pushing |
| `remote origin already exists` | A remote named `origin` is already configured | Inspect with `git remote -v` |
| `src refspec main does not match any` | The branch name may be wrong or there may be no commit | Check `git branch` and `git status` |
| `unmerged paths` | Conflict resolution is incomplete | Resolve the affected files and stage them |
| `All conflicts fixed but you are still merging` | Conflicts are resolved, but the merge has not been concluded | Follow `git status`; create the merge commit if required |
| `refusing to merge unrelated histories` | Local and remote histories were independently created | Use `--allow-unrelated-histories` only if combining the histories is intended |
| `Permission denied` or authentication errors | Authentication or repository permissions may be incorrect | Verify browser login, account access, and remote URL |

### Diagnostic Command Sequence

When you are unsure why a push failed, start with:

```powershell
git status
git remote -v
git fetch origin
git log --oneline --graph --decorate --all -10
```

These commands help identify the current repository state, remote configuration, and commit history.

Do not automatically run `git pull`, `git reset --hard`, or `git push --force` without understanding the situation.

---

## 25. Quick Command Reference

| Command | Purpose |
|---|---|
| `git --version` | Check the installed Git version |
| `git config --global --list` | View global configuration |
| `git init` | Initialize a repository |
| `git clone URL` | Clone a repository |
| `git status` | Check the working tree |
| `git add .` | Stage eligible changes in the current directory |
| `git add -A` | Stage additions, modifications, and deletions across the repository |
| `git commit -m "message"` | Create a commit |
| `git push` | Push commits to the upstream branch |
| `git push -u origin main` | Push and configure the upstream branch |
| `git pull` | Fetch and integrate remote changes |
| `git fetch origin` | Fetch remote updates |
| `git log --oneline` | View compact commit history |
| `git log --oneline --graph --decorate --all -10` | Inspect recent history across branches |
| `git branch` | List local branches |
| `git branch -a` | List local and remote-tracking branches |
| `git switch branch-name` | Switch branches |
| `git switch -c branch-name` | Create and switch to a branch |
| `git remote -v` | View remote URLs |
| `git remote set-url origin URL` | Update a remote URL |
| `git diff` | View unstaged changes |
| `git diff --staged` | View staged changes |
| `git restore FILE` | Discard unstaged changes in a file |
| `git restore --staged FILE` | Unstage a file |
| `git stash` | Temporarily save changes |
| `git stash list` | List saved stashes |
| `git revert COMMIT_ID` | Reverse a commit with a new commit |
| `git merge --abort` | Abort an in-progress merge when appropriate |

---

## 26. Important Notes

- `git add` stages changes; it does not upload them to GitHub.
- `git commit` saves a snapshot in local Git history.
- `git push` uploads local commits to a remote repository.
- `git pull` fetches and integrates remote changes.
- `git fetch` retrieves remote updates without integrating them into the current branch.
- `git status` is a useful first step when troubleshooting Git errors.
- `git log --oneline --graph --decorate --all -10` helps diagnose diverged histories.
- If Git reports `nothing to commit`, there may be no uncommitted changes to save.
- If Git reports `unmerged paths`, resolve conflicts before completing the merge.
- A clean working tree does not necessarily mean your local and remote branches are synchronized.
- Review `.gitignore` before staging generated files, build outputs, IDE files, or other unnecessary content.
- Never commit passwords, API tokens, private keys, or other secrets.
- Authentication and commit author configuration are separate.
- Avoid force-pushing unless you understand its effects on remote history.
- Be careful with destructive commands such as `git reset --hard` and `git clean`.
- If a command fails, read the error message and inspect the repository state before trying additional commands.

---

## 27. Official Documentation

- [Git Documentation](https://git-scm.com/doc)
- [GitHub Documentation](https://docs.github.com/en/get-started)
- [GitHub: About Remote Repositories](https://docs.github.com/en/get-started/git-basics/about-remote-repositories)
- [GitHub: Authentication](https://docs.github.com/en/authentication)
- [GitHub: Resolving Merge Conflicts](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/about-merge-conflicts)

---

*Created as a practical reference for developers learning and using Git and GitHub.*
