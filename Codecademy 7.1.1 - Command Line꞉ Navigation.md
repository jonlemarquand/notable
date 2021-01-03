---
title: 'Codecademy 7.1.1 - Command Line: Navigation'
created: '2019-12-06T14:58:26.817Z'
modified: '2020-02-19T09:56:46.606Z'
---

# Codecademy 7.1.1 - Command Line: Navigation
#pd - codecademy# #command-line

## Your First Command
The command line is a text interface for your computer. It's a program that takes in commands, which it passes on to the computer's operating system to run.

From the command line, you can navigate through files and folders on your computer, just as you would with Finder on Mac OS or Windows Explorer on Windows. The difference is that the command line is fully text-based.
The advantage of using the command line is its power. You can run programs, write scripts to automate common tasks, and combine simple commands to handle difficult tasks – making it an important programming tool.

```
ls
$ ls
2014 2015 hardware.txt
```

2.  In the terminal, first you see $. This is called a shell prompt. It appears when the terminal is ready to accept a command.
3. When you type ls, the command line looks at the folder you are in, and then "lists" the files and folders inside it. The directories *2014*, *2015*, and the file *hardware.txt* are the contents of the current directory.
ls is an example of a command, a directive to the computer to perform a specific task.

## Filesystem

A filesystem organizes a computer's files and directories into a tree structure:

1. The first directory in the filesystem is the root directory. It is the parent of all other directories and files in the filesystem.
2. Each parent directory can contain more child directories and files. Here *blog/* is the parent of *2014/*, *2015/*, and *hardware.txt*.
3. Each directory can contain more files and child directories. The parent-child relationship continues as long as directories and files are nested.

### pwd

* `$ pwd /home/ccuser/workspace/blog`
* pwd stands for "print working directory". It outputs the name of the directory you are currently in, called the working directory.
* Here the working directory is blog/. In Codecademy courses, your working directory is usually inside the home/ccuser/workspace/ directory.
* Together with `ls`, the `pwd` command is useful to show where you are in the filesystem.

cd
$ cd 2015

1. cd stands for "change directory". Just as you would click on a folder in Windows Explorer or Finder, cd switches you into the directory you specify. In other words, cd changes the working directory.
2. The directory we change into is 2015. When a file, directory or program is passed into a command, it is called an argument. Here the 2015 directory is an argument for the cd command.
The cd command takes a directory name as an argument, and switches into that directory.

$ cd ..
To move up one directory, use cd ... Here, cd .. navigates up from jan/memory/ to jan/.

Mkdir
$ mkdir media
The mkdir command stands for "make directory". It takes in a directory name as an argument, and then creates a new directory in the current working directory.

touch
$ touch keyboard.txt
The touch command creates a new file inside the working directory. It takes in a filename as an argument, and then creates an empty file in the current working directory.
Here we used touch to create a new file named keyboard.txt inside the 2014/dec/ directory.
