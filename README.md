# Git
This repository will be about getting started with Git. We will begin by explaining some background on version control tools, then move on to how to get Git running on your system and finally how to get it set up to start working with. At the end of this session you should understand why Git is around, why you should use it and you should be all set up to do so.

##What is git
Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later.

##Local Version Control Systems

Many people’s version-control method of choice is to copy files into another directory (perhaps a time-stamped directory)

![alt tag](https://git-scm.com/figures/18333fig0101-tn.png)

Figure 1-1. Local version control diagram.

##Centralized Version Control Systems

The next major issue that people encounter is that they need to collaborate with developers on other systems.
These systems, have a single server that contains all the versioned files, and a number of clients that check out files from that central place. For many years, this has been the standard for version control (see Figure 1-2).

![alt tag](https://git-scm.com/figures/18333fig0102-tn.png)

Figure 1-2. Centralized version control diagram.

##Distributed Version Control Systems

In a DVCS (such as Git, Mercurial, Bazaar or Darcs), clients don’t just check out the latest snapshot of the files: they fully mirror the repository. Thus if any server dies, and these systems were collaborating via it, any of the client repositories can be copied back up to the server to restore it. 
Every checkout is really a full backup of all the data (see Figure 1-3).

![alt tag](https://git-scm.com/figures/18333fig0103-tn.png)

Figure 1-3. Distributed version control diagram.

#Git Basics

##Snapshots, Not Differences

Conceptually, most other systems store information as a list of file-based changes. 
These systems (CVS, Subversion, Perforce, Bazaar, and so on) think of the information they keep as a set of files and the changes made to each file over time, as illustrated in Figure 1-4.

![alt tag](https://git-scm.com/figures/18333fig0104-tn.png)

Figure 1-4. Other systems tend to store data as changes to a base version of each file.

Git thinks of its data more like a set of snapshots of a mini filesystem. Every time you commit, or save the state of your project in Git, it basically takes a picture of what all your files look like at that moment and stores a reference to that snapshot. 
To be efficient, if files have not changed, Git doesn’t store the file again—just a link to the previous identical file it has already stored. Git thinks about its data more like Figure 1-5.

![alt tag](https://git-scm.com/figures/18333fig0105-tn.png)

Figure 1-5. Git stores data as snapshots of the project over time.

##Git Has Integrity

Git is check-summed before it is stored and is then referred to by that checksum. This means it’s impossible to change the contents of any file or directory without Git knowing about it. This functionality is built into Git at the lowest levels and is integral to its philosophy. 
You can’t lose information in transit or get file corruption without Git being able to detect it.

The mechanism that Git uses for this checksumming is called a SHA-1 hash. This is a 40-character string composed of hexadecimal characters (0–9 and a–f) and calculated based on the contents of a file or directory structure in Git. 
A SHA-1 hash looks something like this:
```javascript
24b9da6552252987aa493b52f8696cd6d3b00373
```

##Git Generally Only Adds Data

Nearly all actions in Git only add data to the Git database. It is very difficult to get the system to do anything that is not undoable or to make it erase data in any way.
This makes using Git a joy because we know we can experiment without the danger of severely screwing things up.

##The Three States

Git has three main states that your files can reside in: committed, modified, and staged. 
Committed means that the data is safely stored in your local database. 
Modified means that you have changed the file but have not committed it to your database yet. 
Staged means that you have marked a modified file in its current version to go into your next commit snapshot.

This leads us to the three main sections of a Git project: the Git directory, the working directory, and the staging area.

![alt tag](https://git-scm.com/figures/18333fig0106-tn.png)

Figure 1-6. Working directory, staging area, and Git directory.

The basic Git workflow goes something like this:

1. You modify files in your working directory.
2. You stage the files, adding snapshots of them to your staging area.
3. You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.

#First-Time Git Setup

## gitconfig

Git comes with a tool called git config that lets you get and set configuration variables that control all aspects of how Git looks and operates. These variables can be stored in three different places:

* ```/etc/gitconfig``` file: Contains values for every user on the system and all their repositories. If you pass the option ```--system``` to ```git config```, it reads and writes from this file specifically.
* ```~/.gitconfig``` file: Specific to your user. You can make Git read and write to this file specifically by passing the ```--global``` option.
* config file in the Git directory (that is, ```.git/config```) of whatever repository you’re currently using: Specific to that single repository. Each level overrides values in the previous level, so values in ```.git/config``` trump those in ```/etc/gitconfig```.

##Your Identity

You can set your user name and e-mail address.This is important because every Git commit uses this information, and it’s immutably baked into the commits you pass around:

``` python
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com 
```

Once you pass the --global option, because then Git will always use that information for anything you do on that system.
If you want to override this with a different name or e-mail address for specific projects, you can run the command without the --global option when you’re in that project.

##Your Editor

By default, Git uses your system’s default editor, which is generally Vi or Vim. If you want to use a different text editor, such as Emacs, you can do the following:
``` python
$ git config --global core.editor emacs
```

##Your Diff Tool

Another useful option you may want to configure is the default diff tool to use to resolve merge conflicts. Say you want to use vimdiff:
``` python
$ git config --global merge.tool vimdiff
```

##Checking Your Settings

If you want to check your settings, you can use the git config --list command to list all the settings Git can find at that point:
``` python
$ git config --list
user.name=Scott Chacon
user.email=schacon@gmail.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```

##Getting Help

If you ever need help while using Git, there are three ways to get the manual page (manpage) help for any of the Git commands:
``` python
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

#Getting a Git Repository

##Initializing a Repository in an Existing Directory

If you’re starting to track an existing project in Git, you need to go to the project’s directory and type
``` python
$ git init
```

This creates a new subdirectory named .git that contains all of your necessary repository files — a Git repository skeleton. At this point, nothing in your project is tracked yet.

If you want to start version-controlling existing files (as opposed to an empty directory), you should probably begin tracking those files and do an initial commit.

``` python
$ git add *.c
$ git add README
$ git commit -m 'initial project version'
```

##Cloning an Existing Repository

If you want to get a copy of an existing Git repository — for example, a project you’d like to contribute to — the command you need is ```git clone```.Git receives a copy of nearly all data that the server has. Every version of every file for the history of the project is pulled down when you run ```git clone```.

You clone a repository with ```git clone [url]```.
``` python
$ git clone https://github.com/UKHomeOffice/hof-bootstrap.git
```
That creates a directory named ```hof-bootstrap```, initializes a ```.git``` directory inside it, pulls down all the data for that repository, and checks out a working copy of the latest version. If you go into the new ```hof-bootstrap``` directory, you’ll see the project files in there, ready to be worked on or used

If you want to clone the repository into a directory named something other than grit, you can specify that as the next command-line option:
``` python
$ git clone https://github.com/UKHomeOffice/hof-bootstrap.git mybootstrap
```

That command does the same thing as the previous one, but the target directory is called mygrit.

Git has a number of different transfer protocols you can use. The previous example uses the ```git://``` protocol, but you may also see ```http(s)://``` or ```user@server:/path.git```, which uses the SSH transfer protocol.

#Recording Changes to the Repository

Remember that each file in your working directory can be in one of two states: ```tracked``` or ```untracked```.
```Tracked``` files are files that were in the last snapshot; they can be ```unmodified```, ```modified```, or ```staged```.
```Untracked``` files are everything else — any files in your working directory that were not in your last snapshot and are not in your staging area. 
When you first clone a repository, all of your files will be ```tracked``` and ```unmodified``` because you just checked them out and haven’t edited anything.

As you edit files, Git sees them as modified, because you’ve changed them since your last commit. You stage these modified files and then commit all your staged changes, and the cycle repeats. This lifecycle is illustrated in Figure 2-1.

![alt tag](https://git-scm.com/figures/18333fig0201-tn.png)

Figure 2-1. The lifecycle of the status of your files.

##Checking the Status of Your Files

The main tool you use to determine which files are in which state is the git status command. If you run this command directly after a clone, you should see something like this:
``` python
$ git status
On branch master
nothing to commit, working directory clean
```

##Tracking New Files

In order to begin tracking a new file, you use the command ```git add```. To begin tracking the ```README``` file, you can run this:
``` python
$ git add README
```

##Viewing Your Staged and Unstaged Changes

If the ```git status``` command is too vague for you — you want to know exactly what you changed, not just which files were changed — you can use the ```git diff``` command.
That command compares what is in your working directory with what is in your staging area. The result tells you the changes you’ve made that you haven’t yet staged.

##Committing Your Changes

Now that your staging area is set up the way you want it, you can commit your changes. Remember that anything that is still unstaged — any files you have created or modified that you haven’t run ```git add``` on since you edited them — won’t go into this commit. They will stay as modified files on your disk. In this case, the last time you ran ```git status```, you saw that everything was staged, so you’re ready to commit your changes. The simplest way to commit is to type ```git commit```

Alternatively, you can type your commit message inline with the ```commit``` command by specifying it after a ```-m``` flag, like this:

##Skipping the Staging Area

If you want to skip the staging area, Git provides a simple shortcut. Providing the ```-a``` option to the ```git commit``` command makes Git automatically stage every file that is already tracked before doing the commit, letting you skip the ```git add``` part:
``` python
git commit -a -m 'added new IP table fix'
```

##Moving Files

If you rename a file in Git, no metadata is stored in Git that tells it you renamed the file. However, Git is pretty smart about figuring that out after the fact — we’ll deal with detecting file movement a bit later.

Thus it’s a bit confusing that Git has a ```mv``` command. If you want to rename a file in Git, you can run something like
``` python
$ git mv file_from file_to
```

However, this is equivalent to running something like this:
``` python
$ mv README README.txt
$ git rm README
$ git add README.txt
```

#Viewing the Commit History

After you have created several commits, or if you have cloned a repository with an existing commit history, you’ll probably want to look back to see what has happened. The most basic and powerful tool to do this is the ```git log``` command.

``` python
commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit
```

One of the more helpful options is ```-p```, which shows the diff introduced in each commit. You can also use ```-2```, which limits the output to only the last two entries.

#Using a GUI to Visualize History
GitKraken demo...

##Showing Your Remotes
To see which remote servers you have configured, you can run the ```git remote``` command. It lists the shortnames of each remote handle you’ve specified. If you’ve cloned your repository, you should at least see origin — that is the default name Git gives to the server you cloned from

You can also specify ```-v```, which shows you the URL that Git has stored for the shortname to be expanded to

##Pushing to Your Remotes
When you have your project at a point that you want to share, you have to push it upstream. The command for this is simple: git push ```[remote-name] [branch-name]```. If you want to push your master branch to your ```origin``` server (again, cloning generally sets up both of those names for you automatically), then you can run this to push your work back up to the server:
``` python
$ git push origin master
```

#Summary
At this point, you can do all the basic local Git operations — creating or cloning a repository, making changes, staging and committing those changes, and viewing the history of all the changes the repository has been through.

