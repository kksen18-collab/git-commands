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

Revert changes of commit by creating a new commit that includes all the changes that have to be made to undo the changes of the other commit.
```shell
git revert <commit-id>
```








  
