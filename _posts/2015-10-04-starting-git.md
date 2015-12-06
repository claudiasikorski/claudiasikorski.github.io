---
layout: post
title: Getting Started with Git
tags:
- intro
- beginner
- git
- linux
- command line
categories:
- Git
- Linux
---
{% include JB/setup %}

# What is Git?

`git` is a distributed version control system.  TO: write in own words

# Installing and Configuring Git

I started out with Windows but I quickly realized that Windows is terrible so I dual booted with Linux Mint TODO: put link to how to dual boot.  

After linux mint was installed, installing `git` was as simple as:

    sudo apt-get install git

After confirming that I wanted to installed, it was installed!  As simple as that.  If you wanted to improve your terminal experience, consider configuring linux mint according to [this guide] (TODO)

I then created a free account on [github](http://github.com) as seen [here](https://github.com/claudiasikorski/).


I then needed to configure git on my local computer running the following commands in terminal:

    git config --global user.email "me@myemailprovider.com"
    git config --global user.name "github username"

# Pushing my first commit

## Creating a Repository and adding code 

Frist I clicked the plus right at the top of github, and then clicked new repository.    

I then followed the following instructions to create my notebook repository:     

Create a Readme File in markdown (todo put link to markdown guide)

    echo "# Notebook" >> README.md

Initilize the folder with git:

```bash
    git init
```
See the status of the files being tracked by `git`

```
    git status
```


Add README to be "staged" for commit

```
    git add README.md
```

See status again

```
    git status
```
## Committing code

Since the file has been added I am now ready to commit!

```
    git commit -m "enter commit messaged like initial commit"
```

**Note** the -m means message and is followed by the commit message.  If I run `git commit` then this crazy text editor called vim will open and most probably will have trouble closing it, causing me to quit linux, quit vim and quit using computers.  Luckily for me I know that if I type `git commit` and vim opens, I can easy quit by typing `:q`.  Once I do that, I can commit like I usually do.  I know that I will eventually learn to use vim well, but until I learn the basics of coding, I am going to use [sublimetext] (http://www.sublimetext.com/).  This way I can focus on learning to code while learning linux and getting used to the command line without being discouraged.

## Adding and pushing to remote

After I commited, I am now ready to set my `remote` repository as `origin`.  `remote` is a server location which I will push and pull my code from ("the cloud") and am naming the primary `remote` as `origin`.  I am using github for this since I like the social aspects of the interface and sharing code with others.

```
    git remote add origin https://github.com/claudiasikorski/notebook.git
```

I only need to add `origin` as the `remote` once.  I like to think of `origin` as the name of a place (like "my home") which has a specific location (like an address)  

Once the origin is added, I can run:

```
    git push  origin master
```


This means I am telling git to push (submit my code) to origin (location declared before) on the `master` (main) branch).  Since the repository is simple, I only need one branch which is by default called `master`

And that is it!  I have added, commited and pushed my first commit to github!

# Pushing new edits

Coming soon! +++++













