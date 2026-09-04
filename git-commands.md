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
### 3. Unstaged file modification:
```
git status
On branch feat/commercial/fsd/common_branch
Your branch is up to date with 'origin/feat/commercial/fsd/common_branch'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   config/canonical/s4_sales_invoices.yml

no changes added to commit (use "git add" and/or "git commit -a")
```
#### 1. Check what are all the files that are changed
```
git diff --stat
commercial/foundation/config/canonical/s4_sales_invoices.yml | 2 +-
1 file changed, 1 insertion(+), 1 deletion(-)
```
#### 2. Check what are the changes
```
git diff config/canonical/s4_sales_invoices.yml
diff --git a/commercial/foundation/config/canonical/s4_sales_invoices.yml b/commercial/foundation/config/canonical/s4_sales_invoices.yml
index 23f5a6f..5cd5fd2 100644
--- a/commercial/foundation/config/canonical/s4_sales_invoices.yml
+++ b/commercial/foundation/config/canonical/s4_sales_invoices.yml
@@ -130,7 +130,7 @@ s4_sales_invoices:
     incoterms_part_2:                 sdo.inco2
 
     # --- Dates (seq 501-503) ---
-    created_on_date:                  sdi.erdat
+    created_on_date:                  sdi.erdata
     invoice_date:                     sdi.fkdat
     last_changed_on_date:             sdo.aedat
```

#### 4. If the changes made are as intended, stage them
```
git add commercial/foundation/config/canonical/s4_sales_invoices.yml
```
##### If the changes that are staged are unintended, need to unstage the changes (but the changes do exist in the files)
```
git restore --staged config/canonical/s4_sales_invoices.yml # single file
git restore --staged . # all staged files
```
```
git restore . # changes made after staged version is discarded
```
##### Restore back the working directory to previous commit (does not touch the staging area)
- Ignore the staged changes, unstaged changes will be reverted back
```bash
git restore --source=HEAD .
# or
git checkout HEAD -- .
```
##### Restore back the working directory and staging area to previous commit
- Resets both the staging area and entire repository to the HEAD
```
git reset --hard HEAD
```

#### 5. Commit the change with a message
```
git commit -m "Modified a field mapping name"
```
##### Uncommit but keep everything staged
```
git reset --soft HEAD~1
```
##### Uncommit, unstage but keep every changes in the working directory
```
git reset --mixed HEAD~1
git reset HEAD~1
```

##### Fully destroy the commit and go back to the previous version (nothing staged, nothing changed)
```
git reset --hard HEAD~1
```
##### To check the commits before pushing
```
# per commit
git log origin/feat/commercial/fsd/common_branch..HEAD
git log --name-only origin/feat/commercial/fsd/common_branch..HEAD # list of files changed per commit
git log --name-status origin/feat/commercial/fsd/common_branch..HEAD
git log --stat origin/feat/commercial/fsd/common_branch..HEAD
# total diff with HEAD
git diff --name-status origin/feat/commercial/fsd/common_branch..HEAD # Gives just the name of the files changed
git diff --stat origin/feat/commercial/fsd/common_branch..HEAD # One line summary for each file
```



