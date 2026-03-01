**Create a new git local repo**<br>
```shell
git init
```

**Add a file to stage**<br>
```shell
git add <file-name>
```

**Save a snapshot of your staged changes to the local repository's history**<br>
```shell
git commit -m "<commit message>
```

**See the commits in the Logs**<br>
```shell
git log
``` 

**HEAD** is a pointer that tells Git "where you currently are" in the repository's history.<br>
Example: <commit-id> (HEAD->main)<br>
- main is the branch name.<br>

**Move to another commits**<br>
```shell
git checkout <commit-id>
```
**HEAD** is now detached because commits are loaded outside of a branch.<br>

**Undo Commits**

Revert changes of commit by creating a new commit that includes all the changes that have to be made to undo the changes of the <commit-id>
The key point — git revert targets what a specific commit changed, not everything after it
```shell
git revert <commit-id>
```

**Reset**

git reset moves HEAD back to a particular commit - deleting every commit after it thus re-writing history.
```shell
git reset --hard <commit-id>

git reset --mixed <commit-id> #Remaining changes are staged

git reset --soft <commit-id> #Remaining changes are unstaged>
```
**Create new branch**

```shell
git branch <branch-name>
git checkout <branch-name>

(or)

git checkout -b <branch-name>
```
**Delete branch**

```shell
git branch -d <branch name>
```
**Merging Strategies**

Starting point for all examples:
main:     A --- B --- C
                \
branchA:         D --- E

1. Merge Commit git merge branchA
main:     A --- B --- C ------- M
                \             /
branchA:         D --- E -----

2. Fast-Forward Merge
```shell
git merge --ff-only branchA
```
(only works if main has no new commits since branching)

Before:
main:     A --- B
                \
branchA:         D --- E

After:
main:     A --- B --- D --- E

3. Squash and Merge

```shell
git merge --squash branchA
```

main:     A --- B --- C --- DE
                \
branchA:         D --- E (D and E squashed into one)

6. Rebase and Merge

```shell
git rebase main
git merge branchA
```

Step 1 - rebase branchA onto main:

main:     A --- B --- C
                          \
branchA:                   D' --- E'


Step 2 - fast-forward main:
main:     A --- B --- C --- D' --- E'

Summary:
merge commit:  A--B--C-------M   (knot)
fast-forward:  A--B--D--E        (linear, no rewrite)
squash:        A--B--C--DE       (linear, one commit)
rebase:        A--B--C--D'--E'   (linear, rewritten)









  
