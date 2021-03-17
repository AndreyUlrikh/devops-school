# Commands


## Initialization

```sh
mkdir Test
cd Test
ls -a
git init
ls -ltra ./git
```

By default - Working Directory Name is a Repository Name

```sh
git status
git stat
```
show git tips

```sh
touch README.md
git status
```
UNTRACKED

`git add ./README.md` - regexp patterns can be used

`git status`

`git commit -m "initial commit"`
description helps to undestand what's inside this commit, extremly usefull when reading history.

commit HASH - commit ID, if we need to use this commit then ID should be used.

`git status`

## Changing

```sh
echo "This is a Test GIT project" > README.txt
git status
git commit -a -m "Added README file content"
git status
```
Commit here - sends changed file to repo directly (does not work for new files)

## History

`git log`
short HASH - stores first 7 digits

```sh
git log --pretty=format:"%h %cd %an '%s'"
git log --patch -2
```
Show differences between commits

## Remote branches

`origin/master` - differences between local version and remote

`origin` - name can be changed, you can work with different repositories thus many pointers may be used. Shorthand repository name.

`git remote show` - shows what's happening with remote repo

## Merge / Commit

### Fast-forward

```sh
git checkout -b fix13
git status
touch fix13.txt
git add .
git status
git commit -m "Initial commit of fix13 branch"
git log --pretty=format:"%h %cd %an '%s'"
```

`ls -ltrh` - show files in the filesystem for this branch

`git checkout master` 

`ls -ltrh` - show filesystem again (snapshots)

CAUTION: Do not use branches for content splitting.

`git merge fix13` see fast-forward

`git log --pretty=format:"%h %cd %an '%s'"`

### Simple Three-way (recursive)

```sh
git checkout -b fix14
echo "It's Fix 14 branch" > fix14.txt
git add .
git commit -m "Initial commit for fix14 branch"
```

`ls -ltrh` - file is here

`git checkout master` - no file here

`echo "Two branches was added to this repository" >> README.txt` - adding changes

`git commit -a -m "Added the second line into README.txt"`

`git log --graph --oneline --decorate --all`

`git merge fix14` - editor should appear to insert comment

`git log --graph --oneline --decorate --all` - cons: history mixed up

One more branch:

```sh
git checkout -b fix14
echo "It's Fix 15 branch" > fix15.txt
git add .
git commit -m "Initial commit for fix15 branch"
git checkout master
echo "Fix 15 branch was added to the repository" >> README.txt
git commit -a -m "Added the third line into README.txt"
git checkout fix15
echo "It's a second line for the file" >> fix15.txt
git commit -a -m "Second commit for Fix15 branch"
git checkout master
```

`git log --graph --oneline --decorate --all -7` - two branches master with commit and Fix15 with two commits

`git rebase Fix15` - Master << Fix15

`git log --graph --oneline --decorate --all -7`- solid line instead of two

Previous commit disapeared, new commit added - both of them have same comments.

### Why it's recommended not doing rebase ?

First show that all the files exist

`ls -ltrh`

Answer: previous commit hash disapeared (where HEAD was). 
If someone has already created branch from this commit. Then we'll have commits without parents.
Git allows workarounding this. 
Good practice is to have a documented procedure how to edit core files.

### Git branches

```sh
git branch -a
git branch -D fix13 fix14 fix15
git log --graph --oneline --decorate --all
```

pointers disapeared


### Reset

```sh
ls -ltrh
rm -f file1*txt
ls -ltrh
git status
```

```sh
git reset --hard HEAD
ls -ltrh
git status
```

```sh
rm -f file1*txt
git commit -a -m "Wrong commit #1"
git log --pretty=format:"%h '%s'" -3
```

#### Hard

files removed, commit removed

```sh
git reset --hard <HASH>
ls -ltrh
git log --pretty=format:"%h '%s'" -3
```

#### Mixed

commit only removed

```sh
echo "It's a very important thing" > important1.txt
echo "It's an extremely important thing" > important2.txt
git status
git add important1.txt
git commit -m "Wrong commit #2"
git log --pretty=format:"%h '%s'" -3
git reset <LATEST CORRECT COMMIT HASH>
git log --pretty=format:"%h '%s'" -3
```

#### Soft
```sh
git add .
git commit -m "Important commit"
git reset --soft <LATEST CORRECT COMMIT HASH>
git log --pretty=format:"%h '%s'" -3
cat important*txt
git status
```

```sh
git commit -m "Important commit 1"
cat README.txt
git log --pretty=format:"%h '%s'"
git checkout <1st COMMIT HASH> README.txt
```

filesystem remains the same, commits the same, README.txt - replaced

how to return back:

`git reset --hard HEAD` or `git checkout`

```sh
ls -ltrh
cat README.txt
```

#### How to rollback merge

```sh
git checkout -b wrng
cat /dev/null > README.txt
cat README.txt
echo "1111" > wrng.txt
git add .
git commit -m "Wrong"
git checkout master
git merge wrng 
cat README.txt
git log --graph --oneline --decorate --all -6
```

Reflogs
`git reflog -5` all our actions

`git reset --hard HEAD@{1}` - rollback by actions

`git log --graph --oneline --decorate --all -6` - commits still there, files still there, but fast-forward removed

`cat README.txt`


#### Git commit with squash

Just in case change editor:
`git config --global core.editor "vim"`

Make 3 commits
```sh
echo "one" >> file.one
git add  file.one
git commit -m "file.one added"
```

```sh
echo "two" >> file.two
git add  file.two
git commit -m "file.two added"
```

```sh
echo "three" >> file.three
git add  file.three
git commit -m "file.three added"
```

Make interactive rebase 
`git rebase -i HEAD~3`

and select squash / drop / etc. for commits


