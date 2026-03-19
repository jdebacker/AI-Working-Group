# Git and GitHub

## What is Git?

Git is a **version control system** — software that tracks changes to files over time, allowing you to recall specific versions, compare changes, and collaborate with others without overwriting each other's work. Think of it as a detailed "track changes" history for an entire project directory, not just a single document.

Git was created by Linus Torvalds in 2005 and is now the de facto standard for version control in software development and increasingly in research.

## What is GitHub?

[GitHub](https://github.com) is a web platform for hosting Git repositories. It adds a collaborative layer on top of Git: you can share your code publicly or privately, review others' contributions, track issues, and run automated workflows.

For research purposes, GitHub is useful for:

- Keeping a backup of your code and writing
- Sharing replication materials alongside a paper
- Collaborating with coauthors on code
- Tracking the history of an analysis as it evolves

## Core Concepts

### Repository

A **repository** (or "repo") is a directory whose history is tracked by Git. It contains all your project files plus a hidden `.git` folder that stores the full version history.

### Commit

A **commit** is a snapshot of your project at a point in time. Each commit has a message describing what changed and a unique identifier (a hash like `a3f2c19`). Commits are the building blocks of Git history.

### Branch

A **branch** is an independent line of development. The default branch is typically called `main`. Creating a new branch lets you develop a feature or experiment without affecting the stable version of the project. Branches can later be **merged** back together.

### Remote

A **remote** is a version of your repository stored somewhere else — typically on GitHub. The most common remote is named `origin`. You **push** local commits to the remote and **pull** (or **fetch**) changes from it.

### Staging Area

Before committing, you **stage** files you want to include. This lets you commit only a subset of your changes, keeping commits focused and meaningful.

## Basic Workflow

A typical Git workflow looks like this:

```
# 1. Clone an existing repo (first time only)
git clone https://github.com/username/repo-name.git

# 2. Check the current state of your working directory
git status

# 3. Make changes to files, then stage them
git add filename.py
# or stage everything changed:
git add .

# 4. Commit the staged changes with a descriptive message
git commit -m "Add regression analysis for Table 3"

# 5. Push your commits to GitHub
git push origin main
```

If you are starting a brand new project:

```
git init                         # initialize a new repo
git add .                        # stage all files
git commit -m "Initial commit"   # first commit
git remote add origin <url>      # link to GitHub remote
git push -u origin main          # push and set upstream
```

## Working with Branches

```
# Create and switch to a new branch
git checkout -b feature/new-analysis

# Switch between existing branches
git checkout main

# Merge a branch into main
git checkout main
git merge feature/new-analysis

# Delete a branch after merging
git branch -d feature/new-analysis
```

## Common Git Commands Reference

| Command | Description |
|---|---|
| `git status` | Show changed and staged files |
| `git log` | Show commit history |
| `git diff` | Show unstaged changes |
| `git add <file>` | Stage a file |
| `git commit -m "msg"` | Commit staged changes |
| `git push` | Push commits to remote |
| `git pull` | Fetch and merge remote changes |
| `git clone <url>` | Clone a remote repository |
| `git checkout -b <branch>` | Create and switch to a branch |
| `git merge <branch>` | Merge a branch into current |
| `git stash` | Temporarily save uncommitted changes |
| `git stash pop` | Restore stashed changes |
| `git tag <name>` | Tag a commit (e.g., for a paper version) |

## Collaborating on GitHub

### Forking and Pull Requests

When contributing to a repository you do not own, the standard workflow is:

1. **Fork** the repository — this creates your own copy on GitHub
2. Clone your fork locally
3. Create a branch and make your changes
4. Push the branch to your fork
5. Open a **Pull Request (PR)** on GitHub to propose merging your changes into the original repo
6. The maintainer reviews, requests changes if needed, and merges

### Issues

GitHub **Issues** are a lightweight way to track tasks, bugs, or discussion points in a project. For a research project, you might use issues to track tasks like "clean variable names in dataset" or "replicate Table 2."

### .gitignore

A `.gitignore` file tells Git which files to ignore — for example, large data files, compiled outputs, or credentials. A typical entry:

```
# Ignore compiled Python files
__pycache__/
*.pyc

# Ignore data files
data/*.csv
data/*.dta

# Ignore OS files
.DS_Store
```

## Setting Up Git

### Installation

Git comes pre-installed on most macOS and Linux systems. On Windows, install [Git for Windows](https://gitforwindows.org/).

To check if Git is installed:

```
git --version
```

### Configuration

Set your name and email so that commits are attributed correctly:

```
git config --global user.name "Your Name"
git config --global user.email "you@email.com"
```

### Authentication with GitHub

GitHub requires authentication for pushing to repositories. The recommended method is to use **SSH keys** or the **GitHub CLI** (`gh`). See [GitHub's documentation](https://docs.github.com/en/authentication) for setup instructions.

## Further Resources

- [PSL Models Git Tutorial](https://pslmodels.github.io/Git-Tutorial/content/intro.html) — a comprehensive tutorial combining Git fundamentals with open-source project best practices; co-authored by Jason DeBacker
- [CompEcon Git Reference](https://github.com/jdebacker/CompEcon_Fall25/tree/main/Productivity#4-git-and-github-tutorial) — a concise Git command reference from USC's Computational Economics course
- [Git Basics (video)](https://www.youtube.com/watch?v=U8GBXvdmHT4) — a short video introduction
- [Pro Git Book](https://git-scm.com/book/en/v2) — the definitive free reference, available online
- [GitHub Skills](https://skills.github.com/) — interactive GitHub tutorials
