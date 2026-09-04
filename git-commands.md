```bash 
git show d51b5ed89f2d30735e95a5eef66d22f6e13a52a5 --stat
```

- Shows who made the commit, when was it made, the commit message, what are the files affected and linewise stats
Output:
```text
Author: driesdekoker <dries.dekoker@eteximc.com>
Date:   Fri Aug 28 10:27:16 2026 +0200

    fixed merger issues

 .../src/pipelines/foundation_engine.ipynb          | 744 ++++++++++++++++++++-
 1 file changed, 743 insertions(+), 1 deletion(-)
```
## I created a new branch feat/commercial/fsd/MPER3 from feat/commercial/fsd/MPER, the MPER has the latest commit but in MPER3 I need to go to the old commit 3c8c5549 - how to do that:
### Method 1 - Just Checkout an old commit - useful for adhoc testing:
```bash
git checkout 3c8c5549
```
### Method 2 - 
```bash
git checkout feat/commercial/fsd/MPER3
git reset --hard 3c8c5549
git push origin feat/commercial/fsd/MPER3 --force
```
### Method 3 - Create a new branch with Old Commit ID
```bash
git checkout -b feat/commercial/fsd/MPER3-old 3c8c5549
```
## To see git diff only for one file
```bash
git diff feat/commercial/fsd/MPER feat/commercial/foundation/fsi -- commercial/foundation/src/pipelines/config_table.ipynb
```
- This gives the summary of changes of that one file between these 2 branches
```text
 commercial/foundation/src/pipelines/config_table.ipynb | 42 +++++++++++++-----------
 1 file changed, 22 insertions(+), 20 deletions(-)
```
## git status
### 1. Ahead:
```bash
 git status
```
```text
On branch feat/commercial/fsd/MPER
Your branch is ahead of 'origin/feat/commercial/fsd/MPER' by 1 commit.
  (use "git push" to publish your local commits)
nothing to commit, working tree clean
```
- The remote branch does not have the commit that local has, you need to push the commit
### 2. Behind:
```
git status
```
```
On branch feat/commercial/fsd/common_branch
Your branch is behind 'origin/feat/commercial/fsd/common_branch' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
```
- Remote has one commit higher than the local one, you need to pull the commit
