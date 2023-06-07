# Git

Stage all files including untracked files

```
git add -A
```

Stage tracked files and commit

```
git commit -am "message"
```

Amend last commit with the same message

```
git commit --amend --no-edit
```

Amend last commit with a different message

```
git commit --amend -m "message"
```

Git log in one line

```
git log --oneline
```

Git log in one line with a graph

```
git log --graph --oneline --decorate
```

List out all local branches along with the latest commit

```
git branch -vv
```

Uncommit X changes and stage them

```
git reset --soft HEAD~X
```

Uncommit and unstage X changes (changes are left in the working tree)

```
git reset --mixed HEAD~X
```

Uncommit, unstage and remove X changes from the working tree

```
git reset --hard HEAD~X
```

Set a different ssh-key for specifically for the repository

```
git config core.sshCommand "ssh -o IdentitiesOnly=yes -i ~/.ssh/private-key-filename-for-this-repository -F /dev/null"
```

Disable auto-crlf

```
git config --global core.autocrlf false
```
