# Learning Git and Github

## Optional Overview of Version Control

* When working with digital files, whether is a .docx word file or a programming .py Python file, you probably don't finish all your work at once.
* In this case, the software you use to manage and track changes in these files is your **Version Control**.
* **Git** itself is one example of a **VCS**, and while there are others(Subversion,mercurial,etc..),**Git** is by far the most popular **VCS** in the world.
* Fortunately for us **Git** is free and open-source and we can download it to our own computer.
* What is **GitHub**?

    * You can use git completely free locally, and use it to track changes on your local files on your own computer.
    * However if you want to host your code on the internet, its typically easier to use a hosting service like **Github**.

* **Github** Vs **Git**

    * **Git** - open source VCS Software
    * **Github** - Company that operates a service for hosting files on the internet that are managed using git.

# Starting with Git
 
- **Git** typically refers to the entire project and **git** is the actual program used.

- Git itself is hosted on the website https://github.com/git/git

- Github provides many features especially graphical interfaces of many git features.

## Configure Git
 
* Configuration commands 
    * **git config --global user.name "user"**
    * **git config --global user.email "email"**

## Creating and Cloning Repositories

- The place where we track changes and manage our files which are using Git is called **Repository**.

- How can we create a Git Repository?

    * git init - initializes a Git Repository
    * git status - will report back the status of git repository.
    
- git init -> .git -> inside folders

* Create the local repository at command line 
* Then use **git clone** to clone to our local machine.

# Starting to use Git

* A Git command doesn't just pertain to a saving changes in a single file. It can constitute specific changes accross an entire working directory.

* working directory -> Staging area -> Repository -> push 

* Repository ->Working directory(Pull)

* **git commit -m " "**

## Add and Commit

- Syntax : **git add** and check status for **git status** an for commit **git commit**

## Git Log

* it shows all the commits made to the repository
* a history of repo

## Git remote and push

* we can check remote branch using **git remove-v** 
* **git remote add name https://url.git** : to add a remote branch using the git remote command syntax.

* **git remote add origin https://url.git** : We call this remote branch the **origin** branch 

* Once we have connected to our remote branch on github, we can **push** our code to the remote branch.

* we tell git to push to the remote main/master branch called origin with the command 

    * git push -u origin main/master

## Fetch and Pull
Ex                 **add**         **commit**           **Push**
* working directory ---->staging area-------->Local Repo------------>Remote

ex            **fetch**
* Remote Repo -------->Local repo(git fetch)


# Branches and working with others

* Branches allows us to organize a repository and split it apart so multiple people can work on it or so s solo devloper can work on different aspects of a project on a seperated work.
* As we need incorporate the workflows of others or be able to focus on new updates without breaking old code, we need **BRANCHES**.
* They represent an independent line of development.
* Upon creating new repo with **git init** we create a new branch called **master branch** (or **main branch**).

### Understanding Head

* **Head** has always been pointing to most recent commit in master branch 
    * **HEAD -> master**

## Merging Branches

* Git will create a commit(Auto commit) for us. It will also request for you to name the commit, with a default name of **"Merge branch branch_name"**

## Git Diff

* **git diff** which displays the differences between the original file and unstaged changes.
