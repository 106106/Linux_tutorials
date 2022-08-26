# Introduction To Git

Git is a version control system. 

## Initial Setup

### Install Git

First we need to check if Git is installed and if not install it. The following will show the version of Git if it is install.

```
git --version
```

If Git is not installed, you can install it on APT managed linux systems with the following.

```
sudo apt update && sudo apt install git
```

On DNF managed linux systems you can install Git using:

```
sudo dnf install git
```

### Configure Global Identity Information


```
git config --global user.name "first_name last_name"
git config --global user.email "example@email.ex"
```

### Configure other basic defaults

``` 
git config --global init.defaultBranch main
git config --global core.editor vim
```

### Check Git Setting

To check the configuration setting we just made and where they are stored.

```
git config --list --show-origin
```

## Initialize A Repository

First make or navigate to the folder you wish to monitor with git. This folder will become a Git "repository".

```
mkdir my-git-repo
cd my-git-repo
touch test{1,2,3,4,5}.txt
```

To initialize Git on the folder you are in

```
git init
```

### Check The Status Of Git On Your New Repository

If you had files in the folder you should see them as untracked files.

```
ls
git status
```

## Add Files To A Repository For Tracking

### Start Tracking Existing Files

To start tracking the all files you currently have in the folder containing your Git repository.

```
git add --all
git status
```

Notice the files are now track and are staged to be commited.

### Add a README.md 

A README.md file describes the repository

```
echo "This is a example repository for learning the basic commands of Git" >> README.md
git add README.md
git status
```

## Commit Changes

### Initial Commit

Preform an initial commit for your new repository. This will be the starting point of your version control.

```
git commit -m "Initial project version"
```

### Commit History

View the commit you just made. You can use this to view a history commits for the repository.

```
git log
```

## Create A Branch

### Create and Move To A New branch

Check the status of git again. Notice we are on the main branch.

```
git status
```

Create a new branch and move to that branch

```
git branch my-new-branch
git branch
git checkout my-new-branch
git status
```

### Make Changes To The New Branch

Create some new files and make changes to exiting files.


```
echo "branch test1" >> test1.txt
cat test1.txt
touch branchtest2.txt
ls
git status
git add --all
git status
git branch
git commit -m "Commiting test changes to new branch"
```

### Notice The Differences Between Branchs

```
git branch
ls
grep -iR "branchtest" .
git checkout main
git branch
ls
grep -iR "branchtest" .
git status
```

## Merge A branch

### Create Non-Conflicting Changes On Main

Let's make a change to main which doesn't conflict with the changes on "my-new-branch" for demonstration purposes.

```
echo "non-conflicting change on main" >> test2.txt
cat test2.txt
git add test2.txt
git commit -m "created a non-conflicting change on main"
git status
```

Let's double check the change didn't affect "my-new-branch"

```
git checkout my-new-branch
grep -iR "non-conflicting" .
cat test2.txt
```

### Merge Non-conflicting Branchs

Since "my-new-branch" was a direct child of main and no conflicting changes were made to main we can do a simple "Fast-forward" merge. Make sure we are on main, and then merge "my-new-branch" with main.

```
git checkout main
git branch
git merge my-new-branch
```

## Delete An Unused Branch

Since we've just merged "my-new-branch" with "main" we no longer need "my-new-branch". Let's delete it.

```
git branch -d my-new-branch
git branch
```

## Merge Conflicting Branchs

Let's create a change on main and a new branch "my-second-branch" which conflict and see what happens when we try to merge the two branchs.

### Create A Change On Main

Note: we are skipping the staging environment.

```
echo "conflict on main" >> test3.txt
cat test3.txt
git commit -a -m "Created change on main to conflict with a change on my-second-branch"
git status
```

### Create A Change On "my-second-branch"

Create a new branch, make a change, and commit the change.

```
git branch my-second-branch
git checkout my-second-branch
echo "conflict on my-second-branch" >> test3.txt
cat test3.txt
git commit -a -m "Created change on my-second-branch to conflict with change on main"
git status
git checkout main
git status
```

### Attempt to merger the Conflicting Branchs

```
git merge my-second-branch
git status
```



