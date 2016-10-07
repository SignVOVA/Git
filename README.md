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

