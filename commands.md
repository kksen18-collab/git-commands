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

**Links remote repo to local repo**

```shell
git remote add origin <git url>
```
