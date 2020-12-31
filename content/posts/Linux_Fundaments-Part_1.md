---
title: "Linux Fundamentals - part 1"
date: 2020-12-25T00:25:54-06:00
draft: false
author: "Golgothus"
summary: "TryHackMe Beginner Path - Linux Fundamentals Part 1; Primarily this material runs over beginner Linux commands and usage such as touch, ls, man, su."
categories: ["Linux Fundamentals"]
tags: ["TryHackMe","Beginner Path","Linux"]
featuredImage: "/images/city.png"
---
# Linux Fundamentals - Part 1
## Task 1 - Intro
Getting logged into the box once it's started up.

```bash
ssh shiba1@<vulnerable system IP>
```

## Task 2 - Methodology
Nothing to do here.

## Task 3 - Basic Command Execution
Simple command testing, within your VM. Start by doing `echo` with some text after it to write out to your terminal.

```bash
echo Hello World!
```

## Task 4 - Manual Pages and Flags
In this section we learn about the `man` command. Which is often quite useful for finding the switches a command will take. The below line will open up the man page for the `ssh` command.

```bash
man ssh
```

### Task 4.1 - How would you output hello without a new line
Start by running:

```bash
man echo
```

When we review the switches, we can see that the option for ```-n``` will provide us with the output we desire, a message without a new line.

## Task 5 - ls
Time to review the `ls` command. This command is used to *list* the files within a directory.

Run `man ls` to review the switches / options which can be used to provide and allow for additional functionality when using `ls`.

### Task 5.1 - What outputs all entries
Review the man page for this option, which is `-a`.

### Task 5.2 - What outputs things in a "long list" format
Review the man page, the switch is `-l`

When I review a directory I will often use a combination of the previous switches, `ls -al`, to allow for a better understanding of what files are present.

## Task 6 - cat
cat - Concatenate FILE(s) to standard output.

### Task 6.1 - What flag numbers all output lines
Run `man cat`, sift through the list of switches. We see that `-n` provides line numbers to the output.

## Task 7 - touch
touch - Update the access and modification times of each FILE to the current time.

No tasks for this section we need to answer.

## Task 8 - Running a Binary
| Relative Path | Meaning | Absolute Path | Relative Path | Running a binary with a Relative Path | Running A Binary with an Absolute Path |
|--- | --- | --- | --- | --- | --- |
| . | Current Directory | /tmp/aa | . | ./hello | /tmp/aa/hello |
| .. | Directory before the current directory | /tmp | .. | ../hello | /tmp/hello
| ~ | The user's home directory | /home/\<current user> | ~ | ~/hello | /home/\<user/>/hello |

>export \<varname/>=\<value/> will set that as an environment variable \

It is worth noting that not all commands support the pipe, and some that do support it require you to use - instead of input, for example cat -. So always check to see if the command does support it.

The ; operator works a lot like &&, however it does not require the first command to execute successfully. This means that you can do dkhsgffgsafgfasdgfasfghkgdsgfs; ls and you would still see the output of ls.

### Task 8.1 - How would you run a binary called hello using the directory shortcut
Reviewing the table of contents posted previously in regards to *relative paths* we see that using `.` is our current directory, so using `./` will run a binary from our *current* path. So with this we can safely assume that using `./hello` will run a binary named hello.

### Task 8.2 - How would you run a binary called hello in your home directory using the shortcut ~
Refer to the previous table and we can see that `~` represents our home directory. By using `~/hello` this will run the binary hello, from our home directory.

### Task 8.3 - How would you run a binary called hello in the previous directory using the shortcut ..
Looking back at the table, we see that `..` represents the action of moving to a directory prior to our current directory. Running `../hello` will move one directory up, and run the hello binary found there.

## Task 9 - Binary - Shiba1
While logged into our vulnerable system, we now need to run a binary.

### Task 9.1 What's the password for Shiba2
We can run the following in order to get the password for the shiba2 account:
```bash
touch noot.txt
./shiba1
```

## Task 10 - su
su allows to run commands with a substitute user and group ID.

When called without arguments, su defaults to running an interactive shell as root.

### Task 10.1 - How do you specify which shell is used when you login
Run the good ol' `man su` command. Look through our options, we can see that `-s` will provide us with the specified shell instead of the default, such as /bin/sh, /bin/zsh, etc.

## Task 11
Click complete, time to move on to Linux Fundamentals Part 2!