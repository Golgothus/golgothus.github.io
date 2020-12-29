---
title: "Learn Linux"
date: 2020-12-25T00:25:54-06:00
draft: false
author: "Golgothus"
description: "Test sample description, writing things all DAY LONG!"
summary: "TryHackMe Beginner Path - Learn Linux; Primarily this material runs over beginner Linux commands and usage such as chmod, sudo, grep, find, etc."
categories: ["THM - Beginner Path"]
tags: ["TryHackMe","Linux"]
featuredImage: "/images/cabin.png"
---

# Learn Linux

| Users | Password|
|---|---|
|shiba1 | shiba1 |
|shiba2 | pinguftw|
|shiba3|happynootnoises|
|shiba4|test1234|
|nootnoot|notsofast|

| Relative Path | Meaning | Absolute Path | Relative Path | Running a binary with a Relative Path | Running A Binary with an Absolute Path |
|--- | --- | --- | --- | --- | --- |
| . | Current Directory | /tmp/aa | . | ./hello | /tmp/aa/hello |
| .. | Directory before the current directory | /tmp | .. | ../hello | /tmp/hello
| ~ | The user's home directory | /home/\<current user> | ~ | ~/hello | /home/\<user/>/hello |

>export \<varname/>=\<value/> will set that as an environment variable \

It is worth noting that not all commands support the pipe, and some that do support it require you to use - instead of input, for example cat -. So always check to see if the command does support it

The ; operator works a lot like &&, however it does not require the first command to execute successfully. This means that you can do dkhsgffgsafgfasdgfasfghkgdsgfs; ls and you would still see the output of ls.

Work: \
echo $USER \
export test1234=shiba2

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

