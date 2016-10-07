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

