---
title: Git worktrees
author: Aaron Hallaert
---

Problems
---
<!-- column_layout: [1, 1] -->
<!-- pause -->
<!-- column: 0 -->
Overlapping build directory
___
<!-- pause -->
* plixus-apps r6 - r7 sdk mismatch

![](build_failure.png)

<!-- pause -->
<!-- column: 1 -->

Urgent bugfix / pull request reviews
___

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


Git Worktrees
---

# RTFM

```bash +exec +acquire_terminal
man git-worktree
```

<!-- end_slide -->

Terminology
---

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->
<!-- pause -->
# Working tree
A directory containing all files tracked by git

<!-- pause -->
# .git directory 
Holds the repository's metadata

```bash +exec_replace
ls .git --color=always
```

<!-- pause -->
<!-- column: 1 -->
# Worktree
Working tree + metadata
* typically one main worktree

```bash +exec_replace
ls -alh --color=always
```

<!-- end_slide -->

Git Worktrees Usage
---

```bash +exec
git branch
```

<!-- end_slide -->

# What are branches?
