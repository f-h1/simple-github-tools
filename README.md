![Cover image](https://github.com/f-h1/simple-github-tools/blob/master/front.jpg?raw=true)

# Simple Github Tools (SGT)

A  collection of scripting friendly commands, written in bash, to manage and maintain github repositories effectively, for the minimum requirement of publishing and updating repositories. 

## Table of Contents
- [Intended use](#intended-use)
- [Background](#background)
- [Install](#install)
- [Premiss](#premiss)
- [Requirements](#requirements)
- [Tools](#tools)
  - [Expected arguments for all commands](#expected-arguments-for-all-commands)
  - [sgt-fnd - Find sgt-related git repositories](#sgt-fnd---find-sgt-related-git-repositories)
  - [sgt-clr - Clear files from github](#sgt-clr---clear-files-from-github)
  - [sgt-creadd - Clear git cache and readd](#sgt-gclr---clear-git-cache-and-readd)
  - [sgt-dloc - Delete local cache and local git files](#sgt-dloc---delete-local-cache-and-local-git-files)
  - [sgt-drem - Delete the repository on github.com](#sgt-drem---delete-the-repository-on-githubcom)
  - [sgt-chname - Change name of repository](#sgt-chname---change-name-of-repository)
  - [sgt-upd - Create or update repository](#sgt-upd---create-or-update-repository)
  - [sgt-reset - Reset repo](#sgt-reset---reset-repo)
  - [sgt-sadd - Add submodule](#sgt-sadd---add-submodule)
  - [sgt-sprop - Propagate changes](#sgt-sprop---propagate-changes)
  - [sgt-init - Initiate sgt in directory](#sgt-init---initiate-sgt-in-directory)
  - [sgt-mkcloneurls - Creeate clone urls](#sgt-mkcloneurls---create-clone-urls)
- [Configuration Files](#configuration-files)
  - [repo-name (mandatory)](#repo-name-mandatory)
  - [repo-desc (optional)](#repo-desc-optional)
  - [repo-topic (optional)](#repo-topic-optional)
  - [repo-web (optional)](#repo-web-optional)
  - [repo-visibility (optional)](#repo-visibility-optional)
  - [repo-parents (optional)](#repo-parents-optional)
- [Log Files](#log-files)  
  - [sgt-hist](#sgt-hist)


## Intended use
You are supposed to able to easily make any directory on your computer a github repo and then just forget about it. Then regularly run a script to scan your system for all repo directories in order to sync them against github.com.

- [x] While this is a minimal tool-kit and not very fascinating in itself, the principles behind it, coming alive trough the use of these tools, are fundamental to an intelligent approach to long term creative engagement with computers.

- [x] This is not intended for your complex github projects. Those will remain untouched by this toolkit. However, nothing stops you from adding such projects to the SGT environment to use these tools also with them.

## Background
I'm frustrated that my various creative endeavors never solidify into concrete projects. I don't want to constantly dig through old backups to find poorly written code that I have to rewrite. I also miss having a curated archive of my stuff.

This system is designed to simplify and encourage the creation of solid projects for all my work -- code, configurations, specifications, etc. -- so I can easily return to, reuse, and improve them.

It's all about finding ways to structure everything neatly to solve problems once and keep everything organized for easy reuse and archiving.

I wish I would have spent time working out this solution many years ago, but it's never too late to get intelligent about organizing your stuff.

## Premiss

After many years of "controlled chaos" and "organized chaos," I conclude any approach excused by those terms, is ineffective in long term, and only support passion-driven projects that lives and dies as quickly as the same passion, leaving nothing but a mess and often half-made projects or projects you never ever use the results of -- because you motivation for the project was the thrill of problem solving, yet you had no real direction, even if you may have thought so, but you just wanted something to work on at the moment. Well, that's me at least -- that summarizes many... most... No! it summarizes all of my previous engagements. I really wish I would have been able to save that old ASCII art I made 20 years ago, but it vanished in the gaps between data migration among floppy discs and IDE-hard-drives.

Clean cut organization of material -- organization that is also functional -- is a pleasure to the soul, reflects intelligence, and supports building on previous efforts or becomes material for reflection to see what has been learned since then, and thus furthers greater self-awareness, as the current views are contrasted against the old.

All creative computer engagements should yield results. Those results should be intelligent in the sense they persist useful over time, or at least are felt worth returning to and studying even if just for nostalgic reasons.

Before doing anything creative, a system for archiving things should be made. The archive should allow for easy access and update of existing entries. The archive should be such, that if everything on the local machine is lost, one's entire work can be easily retrieved.


## Requirements
A Linux system, bash.

You need to set up SSH for github. Authentication token not supported.

Create your SSH key as you create any other SSH key.

Add this to ~/.ssh/config

```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_your_key
```

Do this to test your connection: 

```
ssh -T git@github.com
```

You should then see:

´´´
Hi username! You've successfully authenticated, but GitHub does not provide shell access.´´´

## Install

Put the sgt-* files in a directory of your choice and make sure the directory is in your $PATH so you can invoke the scripts anywhere from your shell.


## Tools
Here are all commands



### Expected arguments for all commands

Unless explicitly stated: 

All tools except *sgt-chname*: Accepts a newline separated list of paths to act on, or a file path given as first argument. If no path is given, it defaults to the current working dir.

*sgt-chname* accepts a path given as first argument. If none given, defaults to current working directory.


### sgt-fnd - Find sgt-related git repositories

Find all git-directories added to the SGT environment.

Matches only directories having the file *repo-name*. Thus, other directories are ignored, even if they have git-specific files.

Outputs a new-line separated list of absolute paths.


```bash
# Example 1 of how to use sgt-clr
sgt-fnd

# Example 2
pwd | sgt-fnd

# Example 3
sgt-fnd /my/path/to/search
```

### sgt-clr - Clear files from github

**Warning! Good way to potentially mess up your git.**

Clearing files from github.com based on .gitignore and the global .gitignore file.

Usecase: you discover a file on your github.com page you want to remove. Add the file to your .gitignore (either local or global), then run this command.

For example, if you add myfile.txt to .gitignore and run this command, it will ensure that file is removed from github.com.

```bash
# Example of how to use sgt-clr
cd /path/to/repository
sgt-clr
```

### sgt-creadd - Clear git cache and readd

**Warning! Good way to potentially mess up your git.**

[Currently does not support filepaths by argument and pipe, works in current working directory]

When files that were previously ignored arent being uploaded.

Clearing Git Cache: Sometimes Git's internal cache might need to be cleared to properly track files that were previously ignored.

Will also readd the files.

After this you can run `sgt-upd` again.

```bash
# Example of how to use sgt-clr
cd /path/to/repository
sgt-creadd
```

### sgt-dloc - Delete local cache and local git files

**Warning! Good way to potentially mess up your git.**

Problem you may get

```
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
Failed to push changes to GitHub
```

Run sgt-reset to fix the above.

This command is useless and worse.

Deletes the git-repository metadata from the local directory (not any of your project files). Acts only on local machine.

Example use case: You have caching issues.

Usecase: You want to upload the project under another repo-name (without affecting the cuurent one), you then run this command, then change the repo name in config file *repo-name*.

Usage:

```bash
# Example of how to use sgt-del
cd /path/to/repository
sgt-dloc
```

### sgt-drem - Delete the repository on github.com

Deletes the repo on github.com. Gives you a code and requires you to verify by web-browser.

Acts only on files on github.com.

You may want to run sgt-dloc too, to delete local git repo files (does not remove your non-git files in the target directory).

### sgt-chname - Change name of repository

Changes name of repo on github.com, will also change the name in local repo-name file.

Must abide by github naming convention for repo name.

```bash
# Example of how to use sgt-del
cd /path/to/repository
sgt-chname
```

### sgt-upd - Create or update repository

Synchronizes the repo on your local machine against github.com, pushing your local files to github.com.

Will create repository on github.com if it doesn't already exist.

If you change .gitignore rules (local or global), you need to *sgt-clr*.

The file repo-name must exist. See below how to write it.

Will create the file changelog.md in the local dir if it doesnt exist. You may want to exclude in your gitignore file.

Flags: 
`-m` - add a message to the commit. Without it, it defaults to "Unspecified update [...]"

Usage:

```bash
# Example of how to use sgt-upd
cd /path/to/repository
sgt-upd
```

Add commit message:

```bash
# Example of how to use sgt-upd
cd /path/to/repository
sgt-upd -m "My commit message here"
```

### sgt-reset - Reset repo

**Warning! Good way to potentially mess up your git.**

**Of the different clear potions** this is better for solving situations you don't bother understanding, but be prepared messing it all up and having to delete and recreate your repo using the clear commands.

**This one fixes the mess created by sgt-dloc**

[Currently does not support filepaths by argument and pipe, works in current working directory]

The sgt-reset script resets a Git repository by deleting .git, reinitializing, and setting a new remote.


Warning: will clear commit history as stoerd remotely among other things.

```bash
sgt-reset
```

I use it when i get this error and the pull dont solve it, and I don't bother finding the solution:

```
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'github.com:username/reponame.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. If you want to integrate the remote changes,
hint: use 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```




### sgt-sadd - Add submodule

[Currently does not support filepaths by argument and pipe, works in current working directory]

Adds a new submodule to the git.

Navigate to the git dir to add a submodule to, and do:

```
sgt-sadd git@github.com:[USERNAME]/[REPONAME].git lib/testsub "Test Submodule"

# For example:
sgt-sadd git@github.com:user1/testsub.git lib/testsub "Test Submodule"


```

lib/submodule will automatically be created.

### sgt-sprop - Propagate changes

[Currently does not support filepaths by argument and pipe, works in current working directory]

Propagate change across all parent modules.

When you are in a sub module git, run this command, it will read all paths in repo-parents. It contains the paths of all parents using this git as submodule. For each path, a recursive git update is run to update all its submodules.

You must first update the target submodule against the git-server, as the propagation pulls from there and not the files directly from your local machine.

```
sgt-sprop
```

### sgt-init - Initiate sgt in directory

[Currently does not support filepaths by argument and pipe, works in current working directory]

Creates an .sgt directory, asks for repo name and puts it in .sgt/repo-name.

```
sgt-init
```

### sgt-mkcloneurls - Create clone urls

[Currently does not support filepaths by argument and pipe, works in current working directory]

Takes piped input, a list of paths, for each path check if is in the sgt-enviornment (having the .sgt dir), and if it does, creete a corresponding git clone url.


```
sgt-fnd | sgt-mkcloneurls
```
Example output:

```
git clone git@github.com:username/reponame1.git
git clone git@github.com:username/reponame2.git
```



## Configuration Files

These configuration files relates to the repo meta data. There are one mandatory and four optional.

All configuration files are stored in .sgt/ in the git project root dir.

### repo-name (mandatory)
Contains the GitHub repository's name in the format USERNAME/REPOSITORY. Used to set the remote repository.

The existence of this file implies its directory is in the sgt-environment, meaning the sgt commands will work.

Example
```
TheUsername/my-repo
```

### repo-desc (optional)
Contains a description for the GitHub repository. Used to update or initialize the repository description.

### repo-topic (optional)
Contains topics associated with the GitHub repository, separated by new lines. These are used to update the repository's topics.

Must abide by github naming convention for topics.

Example:
```
topic1
topic2
```

### repo-web (optional)
URL of the website associated with the repository. Used to set or update the repository's website field.

### repo-visibility (optional)
Contains the visibility setting (public or private) for the GitHub repository. Scripts could potentially use this to set or update the repository's visibility.

Defaults to 'private' if file doesn't exist or if anything else than 'public' is specified in it.


### repo-parents (optional)

Defines a list of paths on your local machine of parents (other gits) using this module as submodule. This file should exist in the root directory of the git that is to be submodule to other modules.

This file is used to propagate changes to all parent gits.

Example:
```
/path1/project1
/path1/project2
```

## Log Files

### repo-hist

Logs from sgt actions.
