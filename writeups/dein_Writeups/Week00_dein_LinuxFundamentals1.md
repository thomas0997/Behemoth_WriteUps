# Linux Fundamentals 1

* **FILE NAME FORMAT:** Week00_Username_LinuxFundamentals1
* **Platform:** TryHackMe
* **Difficulty:** Easy
* **Date completed:** 2026-09-21
* **Category:** Linux

## Summary

This room introduced me to the fundamentals of using the Linux terminal. I learned basic commands for identifying myself, navigating directories, searching for files and text, and combining commands to redirect or capture output.

## Steps

1. I first learned the `whoami` command, which checks and tells me who I am on the system. This is important because I will often be changing directories, so I need to determine what I can and cannot do.

2. I learned the `echo` command, which is the `print` equivalent in Kali Linux, stemming from other coding languages, as it outputs the text I provide.

3. I learned commands that allow me to move around the system without using a mouse:

   * `ls` lists the contents of the current directory.
   * `cd` stands for change directory and allows me to move into different directories, such as folders.
   * `cat` displays the contents of a file.
   * `pwd` stands for print working directory and tells me where I currently am in the directory structure.

4. I also learned the `find` and `grep` commands. `find` lets me search for files by their names, while `grep` lets me search for specific text within files or command output. I also learned the `-i` option, which makes a `grep` search case-insensitive. Without it, `grep` is case-sensitive.

5. Lastly, I learned how to combine commands and capture their output:

   * `&` lets me run a command in the background without waiting for it to finish, which is useful for multitasking.
   * `&&` runs the next command only after the previous command finishes successfully.
   * `>` redirects a command's output into a file and overwrites anything that already exists in that file.
   * `>>` also redirects output into a file, but instead of overwriting the existing content, it adds the new content to the bottom.

## Tools used

* **Linux Terminal** - Used to execute commands and interact with the Linux system.
* **`whoami`** - Used to identify the current user.
* **`echo`** - Used to output text.
* **`ls`** - Used to list directory contents.
* **`cd`** - Used to navigate between directories.
* **`cat`** - Used to display file contents.
* **`pwd`** - Used to identify the current working directory.
* **`find`** - Used to search for files.
* **`grep`** - Used to search for specific text.
* **`&` / `&&`** - Used to combine commands and control how they execute.
* **`>` / `>>`** - Used to redirect command output to files.

## Lesson learned

I learned the basic Linux commands needed to navigate and work within the terminal without relying on a mouse. I also learned how commands can be combined and how their output can be redirected, which will be useful for working more efficiently in future Linux and cybersecurity tasks.
