# Linux Challenges

## SSH Creds

| user | password |
|---|---|
|garry|letmein|
|bob|linuxrules|
|alice|TryHackMe123|

## Task 1

Viewed flag1.txt using cat

```bash
cat flag1.txt
```

## Task 2

Remember to cd into Bob's home directory which can be done by either of the following commands:

```bash
cd ~
cd ..
```

To verifiy your primary working directory you can use the command pwd, which should provide us:

```bash
/home/bob
```

## Task 3

To find a user's bash history, change to their home directory, run ls -al

```bash
ls -al
```

We are going to be looking for a file named **.bash_history**, if the file exists, go ahead and cat into this file

```bash
cat .bash_history
```

## Task 4

Search for where cron jobs are created, resource [cyberciti.biz](https://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/)

Run the command:

```bash
crontab -e
```

Grab Flag4, use ctrl + x, to exit the text editor (on Windows)

## Task 5

To **find** flag5, run the following

```bash
find / flag5 | grep "flag5" 2>/dev/null
```

If you want to learn find syntax, you can use either **man find** or search online

## Task 6

Grep through flag6, to find the flag, first two characters are "c9"

```bash
find / "flag6" 2>/dev/null | grep "flag6"
grep "c9" /home/flag6.txt
```

## Task 7

Seach ye ole processes

To do this, run the following command:

```bash
ps aux | grep "flag7"
```

Reference / examples for ps can be found [here](https://www.cyberciti.biz/faq/linux-find-process-name/)

## Task 8

De-compress flag8

Flag8 is a tar file, which is a compressed file in *nix, to decompress run:

```bash
tar -xvf
```

This will provide a verbose output of all files within the archive and output them into your current working directory

## Task 9

Review your hosts file to find flag9

Run:

```bash
cd /etc/
cat hosts
```

## Task 2.10

Find all other users for the system

For this, we will want to locate all accounts for the system, which can be located in /etc/passwd

```bash
cat /etc/passwd
```

## Task 3.1

Flagis located where all aliases should be stored

View the user's .bashrc, as aliases would typically be stored in a separate file

```bash
cat ~/.bashrc
```

Search for aliases

## Task 3.2

Flagis located in MOTD

Run:

```bash
find / "motd" 2>/dev/null | grep "motd"
cd /etc/update-motd.d/
cat 00-header
```

## Task 3.3

Find the difference between two scripts

Run:

```bash
diff script1 script2
```

## Task 3.4

Where are logs typically stored on the file system

Location /var/log

```bash
cd /var/log/
```

## Task 3.5

Can you find information about the system, such as the kernel version etc.

Find Flag15.

A few commands to look through:

```bash
cat /etc/os-release
uname -oar
cat /proc/version
cat /etc/lsb-release
```

You can also do:

```bash
cat /etc/*-release
```

[Resource](https://www.tecmint.com/find-linux-kernel-version-distribution-name-version-number/)

> LSB = Linux Standard Base, does not seem to be present in every distro

## Task 3.6

Flag16 lies within another system mount.

[Reference](https://linuxize.com/post/how-to-mount-and-unmount-file-systems-in-linux/)

Method I used:

- Reviewed the findmnt commmand
- Reviewed fstab
- findmnt --fstab --evaluate
- findmnt --xstab --evaluate
- cat /mount

Turns out the media was mounted to /

```bash
cd /media
```

We see **f** is a directory under /media, we dig further we see /media/f/l

We ran the following to get the list of subdirectories, recursively:

```bash
ls -R
```

## Task 3.7 - Flag17

SSH key is broken, you have access to read flag17 in /home/alice/flag17

```bash
cat /home/alice/flag17
```

Currently a known issue, reviewed in the TryHackMe Discord as of May 8, 2020

## Task 3.8 - Flag18

Read the hidden Flagin Alice's directory

```bash
ls -al
cat .flag18
```

## Task 3.9 - Flag19

Find the Flagon line 2345

```bash
awk 'fnr==2345' flag19
```

[reference](https://www.commandlinefu.com/commands/view/3802/to-print-a-specific-line-from-a-file)

You can also use vim if you're comfortable with it:

```bash
vim +2345 flag19
```

The spot the cursor lands is the flag, bottom right hand corner of the editor, it will show **2345,1**, meaning line 2345, character 1

Copy / highlight this line \
Type **:quit!** to exit vim

## Task 4.1 - Flag20

Find and retrieve flag20

```bash
find / "flag20" 2>/dev/null | grep "flag20"
cat /home/alice/flag20
```

The output from the flag, looks similar to base-64 encoding

```bash
echo flag20 | base64 -d
```

## Task 4.2 - Flag21

Find and inspect flag21.php

```bash
find / "flag21" 2>/dev/null | grep "flag21"
```

I tried cat, but it gave me a response saying there may be more than what we see

```bash
vim /home/bob/flag21.php
```

And Flagachieved, **bingo bongo**

## Task 4.3 - Flag22

Flag22 is a hex file, locate, find, and translate this file

```bash
find / "flag22" 2>/dev/null | grep "flag22"
```

The file is located in /home/alice/flag22

```bash
cat flag22 | xxd -r -p
```

Flagachieved

## Task 4.4 - Flag23

Locate, read and reverse Flag23.

```bash
find / "flag23" 2>/dev/null | grep "flag23"
```

The Flagis located in Alice's home directory

```bash
cat flag23 | rev
```

## Task 4.5 - Flag24

Analyse the Flag24 compiled C program. Find a command that might reveal human readable strings when looking in the source code.

```bash
strings - flag25
```

[Reference](https://serverfault.com/questions/51477/linux-command-to-find-strings-in-binary-or-non-ascii-file)

## Task 4.6 - Flag25

Does not exist

## Task 4.7 - Flag26

Locate and retrieve Flag27, which is owned by the root user.

We use:

> find / -xdev -type f -print0

The reason the above switches were used can be found below:

- -xdev
  - Restrains search from recursively searching other filesystems
- -type f
  - Looking for "file" types
- -print0
  - This is supported by xargs, to allow for the find command to continue its search in case of *unusual* filenames
  - This can be reviewed in *man find*
    - If you are piping the output of find into another program and there is the  faintest  possibility that the  files  which you are searching for might contain a newline, then you should seriously consider using the -print0 option instead of -print.  See the UNUSUAL FILENAMES section for information about how unusual characters in filenames are handled.

> we use **xargs -0** to handle the output received from -print0 in the find section of this command

```bash
find / -xdev -type f -print0 2>/dev/null | xargs -0 grep -E ^4bceb[a-z0-9]{27} 2>/dev/null
```

## Task 4.8 Flag27

Locate and retrieve Flag27, which is owned by the root user.

- Attempted chown
- Attempted chmod
- Checked hint, we were able to see that there are **commands** we can run as sudo

> This is a great command to keep in mind for recon

```bash
sudo -l
```

Here we see we can run /bin/cat as sudo

```bash
sudo /bin/cat /home/flag27
```

And bingo bongo, we got it

## Task 4.9 Flag28

Whats the linux kernel version?

```bash
uname -srm
```

## Task 4.10 Flag29

Find the file called Flag29 and do the following operations on it:

- Remove all spaces in file.
- Remove all new line spaces.
- Split by comma and get the last element in the split.

For this section we use sed to manipulate the file, keep in mind **who is the owner**

```bash
su garry
tr --delete '\n' flag29 | sed 's/ //g' | sed 's/,/\n/g'
```

Breakdown of each section of the sed arguments

- We use tr, as sed is a stream-line editor, meant primarily for one line at a time input [reference](https://stackoverflow.com/questions/1251999/how-can-i-replace-a-newline-n-using-sed/1252010#1252010), sed can be quite buggy when removing lines

```bash
tr --delete '\n' flag29
```

- This is substituting all spaces and replacing them with nothing

```bash
sed 's/ //g'
```

- This section delimits / replaces all , characters with a new line

```bash
sed 's/,/\n/g'
 ```

## Task 5.1 - flag30

Use curl to find flag 30.

Steps for recon:

```bash
ps aux
```

Didn't find anything

Ran netstat, not much useful information, apparently we needed to add some arguments, used the following:

```bash
netstat -anpt
```

- a - list all connections
- n - numeric ports
- t - tcp traffic
- p - shows the PID and name of the program operating on that port

We could then see that we have a few ports open for listening:

```bash
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:3389            0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.1:3350          0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      -
tcp        0    604 10.10.123.58:22         10.8.9.42:34650         ESTABLISHED -
tcp        0      0 10.10.123.58:22         10.10.231.194:38552     ESTABLISHED -
tcp6       0      0 :::80                   :::*                    LISTEN      -
tcp6       0      0 :::22                   :::*                    LISTEN      -
tcp6       0      0 ::1:631                 :::*                    LISTEN      -  
```

With this I reviewed what processes were running on:

- 631: CUPS (printer service)
- 3306: MySQL
- 3350: FindVIATV

After reading the hint, it mentions "Do you have any services running on localhost"

Which we do, I assume in a more realistic situation, we would run curl against localhost, to try and pulldown any useful information such as a hosted website

```bash
curl localhost
```

## Task 5.2 - flag 31

Flag 31 is a MySQL database name.

MySQL username: root
MySQL password: hello

---

Ran **man mysql**, first thing we see is there is a command with parameters we can run to authenticate to a MySQL db

```bash
mysql --user=user_name --password db_name
           Enter password: your_password
```

Run:

```bash
mysql --user root --password
```

Type in the previously provided password, I'm not nearly as familiar with *SQL, so I opened up the help menu:

```bash
\h
\help contents
```

Saw there was there is help section for **administration**:

```bash
help admnistration
```

We see an argument for **SHOW DATABASES**, I reviewed the help for this as well:

```bash
help show databases
```

We see the following syntax is necessary:

mysql> help show databases \
Name: 'SHOW DATABASES'
Description:
Syntax:
SHOW {DATABASES | SCHEMAS} \
[LIKE 'pattern' | WHERE expr]

Tried the following:

```bash
\g show databases like *
```

Went down a MASSIVE rabbit trail, and we just need to run:

```bash
show databases;
```

## Task 5.3 - bonus

Bonus flag question, get data out of the table from the database you found above!

For this section we can make reference to the flag provided in task 5.2. Looking around further for more information on SQL, I found we can do the following to select the database:

```bash
mysql -u root -p
mysql > use database_2fb1cab13bf5f4d61de3555430c917f4
mysql > show tables from database_2fb1cab13bf5f4d61de3555430c917f4
mysql > select * from flags;
```

Essentially through this we use \* just as a wildcard to provide *all* data from the tables - flags

## Task 5.4 - flag32

Using SCP, FileZilla or another FTP client download flag32.mp3 to reveal flag 32.

```bash
sftp alice@<ip>
get flag32.mp3
```

Play the flag to receive the answer

## Task 5.5 - flag33

Flag 33 is located where your personal $PATH's are stored.

I skipped ahead and found flag34, we ended up going to alice's home directory (/home/alice/) and found .profile file, so I ran the following:

```bash
cat /home/*/.profile | grep -i "flag"
```

## Task 5.6 - flag34

Flag 33 is located where your personal $PATH's are stored.

Tried checking /home/*/.bashrc, nothing to be found. Looked online, and searched for "where are PATH locations stored" there was a mentioning of /etc/environment and /etc/profile from [here](https://stackoverflow.com/questions/37676849/where-is-path-variable-set-in-ubuntu).

```bash
cat /etc/environment
```

Provided the information we needed for this tasks flag.

## Task 5.7 - flag35

Find the list of groups on the system

Groups are typically stored within /etc/groups

```bash
cat /etc/groups
```

## Task 5.8 - flag36

Find the user in the *hackers* group, and read flag36

Review the /etc/group file

```bash
cat /etc/group | grep "hacker"
su bob
find / flag36 2>/dev/null | grep -i "flag36"
cat /etc/flag36
```
