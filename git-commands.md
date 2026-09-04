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
