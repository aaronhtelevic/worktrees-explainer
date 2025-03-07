---
title: Git worktrees
author: Aaron Hallaert
---

Problem
---
<!-- column_layout: [1, 1] -->
<!-- pause -->
<!-- column: 0 -->
# Colliding build directory after switching branches 💥 

* plixus-apps: switch between `master_6x` and `master`
<!-- pause -->

```bash {1-7|3,4,6,7}
git stash
git switch master_6x
rm -rf build_pc # or mv
build_scripts pc
git switch master
rm -rf build_pc
build_scripts pc
```

<!-- pause -->
* Buildroot / Yocto
* linux-tcs
* ...

<!-- pause -->

## Clean build 🐢

* plixus-apps takes 20 minutes to build

## Failure ❌
![](build_failure.png)


<!-- end_slide -->
Problem
---

# Colliding build directory 💥 
# Directory-based IDE configuration 🪛

* CMakeTools extension toolchain configuration


<!-- end_slide -->
Problem
---

# Colliding build directory 💥 
# Directory-based IDE configuration 🪛
# Building / Running integration tests ⏳

* Can not checkout other branch when build/tests are running

<!-- end_slide -->
Problem
---

# Colliding build directory 💥 
# Directory-based IDE configuration 🪛
# Building / Running integration tests ⏳
# Compare 2 branches at the same time (not diff) 🔍

* By switching branches you can not view code simultanuously

<!-- end_slide -->
Problem
---

# Colliding build directory 💥 
# Directory-based IDE configuration 🪛
# Building / Running integration tests ⏳
# Compare 2 branches at the same time (not diff) 🔍
# Urgent bugfix / Pull request reviews 🐛


<!-- pause -->

```bash {4|5|9|10|4,11|12|4,11} 
# ...
# uncommitted changes in current directory
# ...
git stash
git checkout bugfix/urgent_bug
# ...
# fix the bug / review the pr / ...
# ...
build_scripts pc
git checkout feature/i_was_working_here
git stash pop
rm -rf build_pc
# resume work
```
<!-- incremental_lists: false -->

<!-- end_slide -->
Problem
---

# Colliding build directory 💥 
# Directory-based IDE configuration 🪛
# Building / Running integration tests ⏳
# Compare 2 branches at the same time (not diff) 🔍
# Urgent bugfix / Pull request reviews 🐛
<!-- end_slide -->

Problem
---

> [!CAUTION]
> Polluted working tree due to context switching

```mermaid +render
    gitGraph
       commit
       commit
       branch master_6x
       commit
       commit tag: "v6.9.43"
       branch bugfix/urgent_bug
       checkout bugfix/urgent_bug
       commit id: "fix: improve"
       checkout master_6x
       commit
       checkout main
       commit id: "SDK changed" tag: "sdk bump"
       commit
       branch feature/i_was_working_here
       checkout feature/i_was_working_here
       commit
       commit
       checkout main
       commit
```

## Working tree
A directory containing all files tracked by git

<!-- end_slide -->


Solution: Git Worktrees
---

# Terminology
<!-- column_layout: [1, 2] -->
<!-- column: 0 -->
## Working tree
A directory containing all files tracked by git

<!-- pause -->

## .git directory 
Holds the repository's metadata

```bash +exec_replace
ls .git --color=always
```

<!-- pause -->
<!-- column: 1 -->
##  Worktree
Working tree + metadata
* typically one main worktree

```bash +exec_replace
ls -alh --color=always
```

<!-- end_slide -->

The manual
---

```bash +exec +acquire_terminal
man git-worktree
```
<!-- pause -->
# Linked worktree 🔗

A worktree that links back to the original worktree / metadata.

```mermaid +render
graph TD;
    subgraph repo_container [main worktree]
       repo[plixus-apps] --> gitdir@{ shape: documents, label: "/.git" };
       gitdir --> gitworktreedir@{ shape: documents, label: "/.git/worktrees/our_worktree"}
    end
    subgraph worktree_container [worktree]
       worktree["(clean working tree) ../our_worktree"] --> gitfile@{ shape: doc, label: "../our_worktree/.git" };
       gitfile --> gitworktreedir;
    end

```

<!-- end_slide -->

Goal
---

<!-- jump_to_middle -->

* Work with multiple branches at the same time

# In other words

* Create a new working tree that does not interfere with our current working tree


<!-- end_slide -->

Git Worktrees Usage
---

<!-- column_layout: [1,1]-->
<!-- column: 1 -->
# git worktree -h
```bash +exec_replace
git worktree -h
```
<!-- pause -->
<!-- column: 0 -->

# Create a new worktree

```bash +exec
cd ~/Developer/televic/plixus-apps
/// git worktree remove our_new_worktree >/dev/null 2>&1 
/// rm -rf ../our_new_worktree
/// git worktree prune
/// git branch --delete branch_for_our_new_worktree -f >/dev/null 2>&1 

git worktree add -b branch_for_our_new_worktree ../our_new_worktree master_6x
```
<!-- end_slide -->
Git Worktrees Usage
---

```bash
git worktree add -b branch_for_our_new_worktree ../our_new_worktree master_6x
```

## Result
<!-- column_layout: [1, 1] -->

<!-- column: 0 -->
```bash +exec
/// cd ~/Developer/televic/plixus-apps
git worktree list
```

<!-- column: 1 -->
```bash +exec
/// cd ~/Developer/televic/plixus-apps
git branch --color=always
```

<!-- pause -->
```bash +exec
/// cd ~/Developer/televic/plixus-apps
git rev-parse master_6x
git rev-parse branch_for_our_new_worktree
```
<!-- reset_layout -->
<!-- pause -->
<!-- end_slide -->
Git Worktrees Usage
---

> [!CAUTION]
> A branch can only be checked out in one worktree

```bash +exec
/// cd ~/Developer/televic/plixus-apps
/// echo "`pwd`"
/// echo ""
git switch branch_for_our_new_worktree
```
<!-- end_slide -->

Hands-on
---

# Colliding build directory
<!-- pause -->
# Simultanuous build

<!-- pause -->
# Inspect several versions (with intellisense)

<!-- pause -->
<!-- end_slide -->

Enhance workflow
---

# VSCode

- Git worktree menu

<!-- pause -->

# Lazygit

<!-- pause -->

# Tmux Integration

- [twt-cli](https://github.com/aaronhallaert/twt-cli.git)
- Review a PR


<!-- end_slide -->
