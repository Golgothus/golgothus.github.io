---
title: "Linux Fundamentals - part 2"
date: 2020-12-25T00:25:54-06:00
draft: false
author: "Golgothus"
summary: "TryHackMe Beginner Path - Linux Fundamentals Part 2; Primarily this material runs over beginner Linux commands and usage such as chmod, sudo, grep, find, etc."
categories: ["Linux Fundamentals"]
tags: ["TryHackMe","Beginner Path","Linux"]
featuredImage: "/images/city.png"
---
# Linux Fundamentals - Part 2
## Task 1 - Intro
In this course we'll be going through a walk-thru of Linux Operators and Advanced File Operators, deploy your machine, and let's login!

No tasks for this section we need to respond to.

## Task 2 - SSH - Intro
Quick run thru of SSH. This tool is going to be our main method for connecting to our vulnerable systems on the TryHackMe platform.

ssh (SSH client) is a program for logging into a remote machine and for executing commands on a remote machine.  It is intended to provide secure encrypted communications between two un‐trusted hosts over an insecure network.  X11 connections, arbitrary TCP ports and UNIX-domain sockets can also be forwarded over the secure channel.

ssh connects and logs into the specified destination, which may be specified as either [user@]hostname or a URI of the form ssh://[user@]hostname[:port]. The user must prove his/her identity to the remote machine using one of several methods (see below).

No tasks for this section we need to respond to.

## Task 3 - PuTTY and SSH
Run through of PuTTY and SSH.

No tasks for this section we need to respond to.

## Task 4 - "&&"
The parameter `&&` is used to concatenate two sets of commands together, and will only execute the command following the `&&` if the first command returns a value of true.

No tasks for this section we need to respond to.

## Task 5 - "&"
The parameter `&` will run a job in the background. Usually you are unable to run a command while it takes its time to execute, but putting it in the background using `&` allows you to run other commands AND execute your command that was put in the background.

Example:
```bash
# this will run an nmap service scan against our vulnerable TryHackMe box and then output the contents of the scan into a file named thm.txt, all while running it in the background
nmap -sS <vulnerable ip image> > thm.txt &
```

## Task 6 - "$"
We are now taking a look at the `$` operator for Linux, this operator refers to environment variables. We are able to create our own environment variables by doing `export <varname>=<value>` and it will create a correlating variable with its associated value. 

## Chmod
> chmod \<file> \<permission>

The first digit controls the permissions for the user that owns the file \
The second digit controls the permission for a group \
The third digit controls permissions for everyone that's not a part of the user or group

| Number | Permission set |
|---|---|
|1 |That file can be executed |
|2 |That file can be written |
|3 |That file can be written to and executed |
|4 |That file can be read |
|5 |That file can be read and executed |
|6 |That file can be read and written to |
|7 |That file can be read, written to, and executed |

### Examples

|Command | Meaning |
|---|---|
|chmod 341 file | The file can be executed and written to by the user  that owns the file - The file can be read by the group that owns the file - The file can be executed by everyone else. |
| chmod 777 file | The file can be read, written to, and executed by the user that owns the file - The file can be read, written to, and executed by the group that owns the file - The file can be read, written to, and executed by everyone else |
|chmod 455 |The file can be read by the user that owns the file - The file can be read and executed by the group that owns the file - The file can be read to and executed by everyone else |

## chmod

Examples:

- chmod user:group file
- chmod shiba2:shiba2 test.txt

You can use chown on just a user, and not add a group, example:

- chown shiba2 file

To recursively operate on every file, use -R

## ln

"Hard linking", which completely duplicates the file, and links the duplicate to the original copy. Meaning What ever is done to the created link, is also done to the original file. The ln syntax is:

ln source destination

The syntax for a symbolic link is the exact same, but it uses the -s flag, so to create a symbolic link, you would run:

ln -s \<file> \<destination>.

## find

Examples:

- find dir -user
  - to list every file owned by a specific user;
- find dir -group
  - you can use find dir -group

## grep

Note: You can search multiple files at the same time, meaning you can theoretically do:

grep \<string> \<file> \<file2>

For instance let's say you know have the file name of test1234, but you don't know where it is on the system. find can be used to list every file on the OS, and grep can be used to find the actual file.

> find / | grep test1234
>
> find / shiba4 | grep shiba4 > findme.txt

## sudo

How would I run whoami as user jen? \
sudo -u jen whoami

How do you list your current sudo privileges(what commands you can run, who you can run them as etc.) \
sudo -l
## adduser | addgroup

dduser \<username> \
addgroup \<groupname>

usermod -a -G \<groups seperated by commas> \<user> \
Meaning if I wanted to add the user noot to b I would run usermod -a -G b noot.

sudo usermod -a -G test test

## Important Files & Directories

/etc/passwd - Stores user information - Often used to see all the users on a system

/etc/shadow - Has all the passwords of these users

    Not showing for obvious reasons

/tmp - Every file inside it gets deleted upon shutdown - used for temporary files

/etc/sudoers - Used to control the sudo permissions of every user on the system -

    [imgur](https://imgur.com/SEtC1rsToo) Long to show

/home - The directory where all your downloads, documents etc are. - The equivalent on Windows is C:\Users\\<user\>

/root - The root user's home directory - The equivilent on Windows is C:\Users\Administrator

    Not showing because of spoiler ;)

/usr - Where all your software is installed

/bin and /sbin - Used for system critical files - DO NOT DELETE

    Too big to show, but contains all of the basic programs needed for linux to function

/var - The Linux miscellaneous directory, a myriad of processes store data in /var

$PATH - Stores all the binaries you're able to run - same as $PATH on Windows

    $PATH is an environment variable that contains all the binaries you're able to execute.

## ps

To view a list of all system processes, you have to use the -ef flag

To kill a process, run the following: \
kill \<PID>

## Pentest Challenge (with pivot)

- Reveiewed /etc/passwd
- Reviewed /etc/shadow
- Reviewed /var/log
  - Found a log file named test1234, with read permissions for shiba2
- Logged in to shiba2, read /var/log/test1234
- Found password for nootnoot, badabing
- User was in sudoers file, used sudo -i to gain root privs

