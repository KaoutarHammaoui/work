# Git Fundamentals – Hello World Project

This repository documents a complete hands-on introduction to **Git version control**, covering repository setup, commit history management, branching, merging, rebasing, remote repositories, and bare repositories.
---

## 1. Setting Up Git

### Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

### Credential Storage (password/token)

```bash
git config --global --credential.helper store
```

---

## 2. Git Commits Basics

### Initialize Repository

```bash
git init
git status
```

### Staging (interactive)

```bash
git add -p
```

---

## 3. History

### Full History

```bash
git log
```

### One‑Line History

```bash
git log --oneline
```

### Controlled Entries

```bash
git log -n 2
git log --before="5m"
```

### Personalized Format

```bash
git log --pretty="%h - %as - %s - %d - [%an]"
```

---

## 4. Check It Out (Restore)

```bash
git restore --source=HEAD~1 .
git restore --source=HEAD~2 .
git restore --source=main .
```

---

## 5. Tag Me

```bash
git tag v1
git push origin v1

git tag v1-beta HEAD~1
git push origin v1-beta

git checkout v1
git tag
```

---

## 6. Changed Your Mind?

### Restore / Unstage / Revert

```bash
git restore work/hello/hello.sh
git restore --staged work/hello/hello.sh
git revert HEAD
```

### Reset & Cleanup

```bash
git tag oops
git checkout v1
git reset --hard v1

git reflog
git log --all --decorate
git reflog expire --expire=now --all
git gc --prune=now
```

### Amend Last Commit

```bash
git add work/hello/hello.sh
git commit --amend --no-edit
git log --oneline
```

---

## 7. Move It

```bash
mkdir lib
git mv work/hello/hello.sh lib/
git commit -m "Moved hello.sh"

git ls-tree HEAD:lib
```

### Makefile

```bash
git checkout main
touch Makefile
git add Makefile
git commit -m "Makefile added"
```

---

## 8. Blobs, Trees, and Commits

### Explore .git Directory

```bash
cd .git/
```

### Objects Inspection

```bash
git rev-parse HEAD
git cat-file -t <hash>
```

### Dumping Directory Tree

```bash
git cat-file -p <commit-hash>
git cat-file -p <tree-hash>

git ls-tree HEAD:lib
git show HEAD:lib/hello.sh
```

---

## 9. Branching

### Create and Switch Branch

```bash
git checkout -b greet
```

### Add Greeter

```bash
cd lib
touch greeter.sh
nano greeter.sh
git add greeter.sh
git commit -m "Greeter added"
```

### Update hello.sh

```bash
nano hello.sh
git add hello.sh
git commit -m "Hello content added"
```

### Compare Branches

```bash
git checkout main
git diff greet main -- Makefile hello.sh greeter.sh
```

### README & Graph

```bash
touch README.md
nano README.md
git add README.md
git commit -m "README added"

git log --oneline --graph --all
```

---

## 10. Conflicts, Merging, and Rebasing

### Merge Main into Greet

```bash
git checkout greet
git merge main
```

### Conflict Scenario

```bash
git checkout main
nano lib/hello.sh
git add lib/hello.sh
git commit -m "Content changed"

git checkout greet
git merge main
git add lib/hello.sh
git commit -m "Conflict resolved"
```

### Rebasing Greet Branch

```bash
git log --oneline --graph
git checkout greet
git reset --hard HEAD~1
git rebase main
# if conflicts occur
git rebase --continue
```

### Merge Greet into Main

```bash
git checkout main
git merge greet
```

---

## 11. Local and Remote Repositories

```bash
git clone hello cloned_hello
cd cloned_hello
git log --oneline --graph

git remote -v
git remote show origin
git branch -a
```

### Fetch & Merge

```bash
cd ../hello
nano README.md
git add README.md
git commit -m "Update README in original"

cd ../cloned_hello
git fetch origin
git log --oneline --graph --all
git checkout main
git merge origin/main
```

### Track Remote Branch

```bash
git checkout -b greet origin/greet
```

### Push to Remote

```bash
git remote add upstream https://github.com/KaoutarHammaoui/work.git
git push upstream main
git push upstream greet
```

---

## 12. Bare Repositories

### Definition

A **bare repository** has **no working directory**. It contains only Git data (`objects`, `refs`, `HEAD`) and is used as a **shared central repository** (GitHub, GitLab).

### Create Bare Repository

```bash
git clone --bare hello hello.git
ls hello.git
```

### Add as Remote & Push

```bash
cd hello
git remote add shared ../hello.git
git remote -v

nano README.md
git add README.md
git commit -m "Update README for shared repository"

git push shared main
```

### Pull from Shared Repo

```bash
cd ../cloned_hello
git pull origin main
```
