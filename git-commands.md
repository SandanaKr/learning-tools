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
##### After hard reset if you still want to get back the changes
```
git reflog
git reset --hard <commit-hash-from-reflog>
```
##### To check the commits before pushing
```
# per commit
git log origin/feat/commercial/fsd/common_branch..HEAD
commit 607e5a030ff3a00294a6395e5fc45e6de41e7865 (HEAD -> feat/commercial/fsd/common_branch)
Author: Sandanakishnan S <sandanakishnan.s@etexgroup.com>
Date:   Fri Sep 4 18:40:00 2026 +0530

    Added the second file for testing

commit bb68c484df19b46798d27aa8409d6fb01f045e51
Author: Sandanakishnan S <sandanakishnan.s@etexgroup.com>
Date:   Fri Sep 4 18:01:04 2026 +0530

    Test

git log --name-only origin/feat/commercial/fsd/common_branch..HEAD # list of files changed per commit
commit 607e5a030ff3a00294a6395e5fc45e6de41e7865 (HEAD -> feat/commercial/fsd/common_branch)
Author: Sandanakishnan S <sandanakishnan.s@etexgroup.com>
Date:   Fri Sep 4 18:40:00 2026 +0530

    Added the second file for testing

commercial/foundation/config/canonical/s4_sales_invoice_lines.yml

commit bb68c484df19b46798d27aa8409d6fb01f045e51
Author: Sandanakishnan S <sandanakishnan.s@etexgroup.com>
Date:   Fri Sep 4 18:01:04 2026 +0530

    Test

commercial/foundation/config/canonical/s4_sales_invoices.yml
git log --name-status origin/feat/commercial/fsd/common_branch..HEAD
commit 607e5a030ff3a00294a6395e5fc45e6de41e7865 (HEAD -> feat/commercial/fsd/common_branch)
Author: Sandanakishnan S <sandanakishnan.s@etexgroup.com>
Date:   Fri Sep 4 18:40:00 2026 +0530

    Added the second file for testing

M       commercial/foundation/config/canonical/s4_sales_invoice_lines.yml

commit bb68c484df19b46798d27aa8409d6fb01f045e51
Author: Sandanakishnan S <sandanakishnan.s@etexgroup.com>
Date:   Fri Sep 4 18:01:04 2026 +0530

    Test

M       commercial/foundation/config/canonical/s4_sales_invoices.yml


git log --stat origin/feat/commercial/fsd/common_branch..HEAD
commit 607e5a030ff3a00294a6395e5fc45e6de41e7865 (HEAD -> feat/commercial/fsd/common_branch)
Author: Sandanakishnan S <sandanakishnan.s@etexgroup.com>
Date:   Fri Sep 4 18:40:00 2026 +0530

    Added the second file for testing

 commercial/foundation/config/canonical/s4_sales_invoice_lines.yml | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

commit bb68c484df19b46798d27aa8409d6fb01f045e51
Author: Sandanakishnan S <sandanakishnan.s@etexgroup.com>
Date:   Fri Sep 4 18:01:04 2026 +0530

    Test

 commercial/foundation/config/canonical/s4_sales_invoices.yml | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

# total diff with HEAD
git diff --name-status origin/feat/commercial/fsd/common_branch..HEAD # Gives just the name of the files changed
M       commercial/foundation/config/canonical/s4_sales_invoices.yml
M       commercial/foundation/config/canonical/s4_sales_invoice_lines.yml
git diff --stat origin/feat/commercial/fsd/common_branch..HEAD # One line summary for each file
 commercial/foundation/config/canonical/s4_sales_invoices.yml | 2 +-
commercial/foundation/config/canonical/s4_sales_invoice_lines.yml | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```
### Reference Logs
- gits log of changes to the references of HEAD
```
git reflog
607e5a0 (HEAD -> feat/commercial/fsd/common_branch) HEAD@{0}: commit: Added the second file for testing
bb68c48 HEAD@{1}: commit: Test
5cc3087 (origin/feat/commercial/fsd/common_branch) HEAD@{2}: commit: Added the desc columns
28cb31f HEAD@{3}: pull: Fast-forward
```
```
git reset --hard bb68c48
HEAD is now at bb68c48 Test
```
```
git reflog
bb68c48 (HEAD -> feat/commercial/fsd/common_branch) HEAD@{0}: reset: moving to bb68c48
607e5a0 HEAD@{1}: commit: Added the second file for testing
bb68c48 (HEAD -> feat/commercial/fsd/common_branch) HEAD@{2}: commit: Test
```
```
git reset --hard 5cc3087
HEAD is now at 5cc3087 Added the desc columns
```
```
git status
On branch feat/commercial/fsd/common_branch
Your branch is up to date with 'origin/feat/commercial/fsd/common_branch'.

nothing to commit, working tree clean
```
### Local to local new branch creation:
```
git checkout -b feat/commercial/fsd/MPER4 feat/commercial/fsd/MPER3
git push -u origin feat/commercial/fsd/MPER4
# or
git switch -c feat/commercial/fsd/MPER4 feat/commercial/fsd/MPER3
git push -u origin feat/commercial/fsd/MPER4
```
### Origin to local new branch creation:
git fetch origin
git checkout -b feat/commercial/fsd/MPER4 origin/feat/commercial/fsd/MPER3
git push -u origin feat/commercial/fsd/MPER4
# or
git switch -c feat/commercial/fsd/MPER4 origin/feat/commercial/fsd/MPER3
git push -u origin feat/commercial/fsd/MPER4
### Git fetch
```bash
git fetch origin # fetches the remote references to local without changing the working directory
git fetch origin +refs/heads/*:refs/remotes/origin/* # low level fetch spec
git fetch 
git fetch origin <branch> # fetches only that branch from the remote origin
git fetch --all
git fetch --prune # cleans up the local references of deleted branches
git fetch --tags # fetches along with tags; tags are fixed name pointer to a specific commits, used to point the release versions like say git tag v1.0.0
git fetch --dry-run # shows what would be fetched
# Ex. git fetch --dry-run --prune origin - example usable before destructive change
From https://github.com/org/repo
 - [deleted]         (none)     -> origin/feat/old-experiment
   a1b2c3d..e4f5g6h  main       -> origin/main
```
### Git pull
- git pull = git fetch + git merge or git rebase - based on the config
```
git pull origin main
git pull
git pull --rebase
git pull --ff-only
git pull --no-commit
git pull --tags # tags are fixed name pointer to a specific commits, used to point the release versions
git pull --all
```
### Folking
- Suppose you want to contribute to an open source project, below are the steps
  1. Folk their repo (creates the personal copy of someone elses repo)
  2. Clone the fork to your machine
  3. Make changes and commit to your fork
  4. Create a pull request from your folk to the original repo so that the owners review and merge your changes
 
### Step 1 — Folk the repo
### Step 2 — Clone
Ex.: 
```
git clone https://github.com/yourname/linux.git
cd linux
```
At this point, git sets up one remote "origin" pointing to your name (the folked repo)

### Step 3 — But also you need to be in sync with the original project
```
git remote add upstream https://github.com/torvalds/linux.git
git remote -v
origin    https://github.com/yourname/linux.git (fetch)
origin    https://github.com/yourname/linux.git (push)
upstream  https://github.com/torvalds/linux.git (fetch)
upstream  https://github.com/torvalds/linux.git (push)
```
- At this point there are 2 remotes, origin and upstream
### Step 4 — Get the latest changes from the original project
```
git fetch upstream
```
### Step 5 — Bring those changes into your local branch
```
git checkout main
git merge upstream/main
```
### Step 6 — Push the updated main to your own fork
```
git push origin main
```
### Step 7 — Update your feature branch with the latest main (resolve conflicts with YOUR changes here)
```
git checkout feature/my-change
git merge main
```
```
# resolve conflicts in each flagged file
git add <resolved-file>
git commit
```
### Step 8 — Push your feature branch to your fork
```
git push origin feature/my-change
```
### Step 9 — Open the Pull Request
Set: base repository = original project, base branch = main ← head repository = your fork, compare branch = feature/my-change.
### Summary:
```
git fetch upstream
git checkout main
git merge upstream/main
git push origin main

git checkout feature/my-change
git merge main              # resolve conflicts here if any
git push origin feature/my-change
# then open PR on GitHub UI
```
### Git Merge:
```
git checkout feature
git merge main                  # standard merge, creates merge commit if diverged
git merge --no-ff main          # force a merge commit even if fast-forward is possible
git merge --ff-only main         # only merge if it can fast-forward; fail otherwise (no merge commit ever)
git merge --squash main          # combine all incoming commits into one, but don't commit yet — you commit manually after
git merge --abort                # cancel a merge that's stuck in conflict
```
### Git rebase
git checkout feature
git rebase main                  # replay feature's commits on top of main
git rebase -i main               # interactive rebase — reorder, squash, edit, drop commits along the way
git rebase --continue            # after resolving a conflict during rebase, continue replaying remaining commits
git rebase --abort                # cancel the rebase, go back to how things were before starting
git rebase --onto newbase oldbase feature   # advanced: move a branch's commits onto a different base entirely
