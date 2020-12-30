# OWASP Top 10

This room broken down into what seems like several days of material. We'll try and do our best to go through and "update" as we go along.

## [Task 6] [Day 1] Command Injection Practical

This section covers Command Injection. Examples given are SQLi (SQL Injection) and arbitrary command execution. These can be defined as follows:

|Term|Definition|
|---|---|
|SQLi|This occurs when user controlled input is passed to SQL queries. As a result, an attacker can pass in SQL queries to manipulate the outcome of such queries. |
|Command Execution| This occurs when user input is passed to system commands. As a result, an attacker is able to execute arbitrary system commands on application servers.|

Reference for reverse shells for code execution:
[Pentest Monkey](https://www.pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)

Some provided examples of commands we can try executing if we think there is a shell present:

Linux

- whoami
- id
- ifconfig/ip addr
- uname -a
- ps -ef

Windows

- whoami
- ver
- ipconfig
- tasklist
- netstat -an

### Task 6 - Question 1: What strange **text** file is in the website root directory

We access the PHP reverse shell that is provided for us to start running through some normal commands we might use on an already open and available system.

For this we can use the command:

```bash
ls
```

We can even add switches to it if need be, such as:

```bash
ls -al
```

Sift through the available files in this location and provide the answer that seems **most obvious**. The files / directories we are provided with are separated by spaces.

### Task 6 - Question 2: How many non-root/non-service/non-daemon users are there

This was a fun one. It took me a minute, but I remembered that we can review the shadow file in order to see what users / accounts have /bin/bash access to a login shell on the system.

In the PHP Reverse shell I ran:

```bash
cat /etc/passwd
```

Then took the results into Visual Studio Code to try and space things out for easier review. Remember, we're looking for users who are **non-root/non-service/non-daemon** accounts.

Reference and review to for login shell and "listing users in Linux": [Linuxize Blog](https://linuxize.com/post/how-to-list-users-in-linux/)

### Task 6 - Question 3: What user is this app running as

Pretty simple question for us here, just run:

```bash
whoami
```

### Task 6 - Question 4: What is the user's shell set as

This kind of takes us back to question 2 where we reviewed who all has a login shell, or shell access on the system.

We can easily bring back up the /etc/passwd file and review

```bash
cat /etc/passwd
```

Look for the type of "shell" this user has and we're all set!

### Task 6 - Question 5: What version of Ubuntu is running

For this question we can review the notes from a previous room we've done - [Linux Challenges](linux_challenges.md)

Long story short, we can use the following command to find the version information:

```bash
cat /etc/*-release
```

### Task 6 - Question 6: Print out the MOTD. What favorite beverage is shown

Another callback to [Linux Challenges](linux_challenges.md) where we see that the MOTD can be located by running:

```bash
cd /etc/update-motd.d/ && cat 00-header
```

## [Task 8] [Day 2] Broken Authentication Practical

### [Task 8] - Question 1: What is the flag that you found in darren's account

This was an interesting question to determine the *why* this exploit works. What I ended up doing was intercepting the traffic when I logged into my user " darren". What Burp Suite showed me was that it url-encoded the " " to be equivalent to "+darren".

[Burp Suite Intercept](https://imgur.com/xFI1Pwu.png)

With SQL not being set to properly sanitize my input, it ended up making a new user " darren" WITH the imported account settings from "darren".

## [Task 12] [Day 3] Sensitive Data Exposure (Challenge)

Supporting material:

Some databases can be stored in a flat-file, this format might usually be an SQLite file. To interact with this flat-file database we can run through the following:

```bash
$ sqlite3 <database name>.db
```

From here we can see the tables in the database by using the .tables command:

```bash
sqlite > .tables
customers
```

We can then see what information is *inside* the table in order to help us further query our data.

```bash
PRAGMA table_info(customers);
```

A useful online tool for cracking simple hashes - [crackstation](https://crackstation.net/)

### [Task 12] - Question 1: The developer has left themselves a note indicating that there is sensitive data in a specific directory. What is the name of the mentioned directory

I ended up finding this by browsing around in the source code on the home page. Looks like we are able to access the sites assets directly. However, I intended to also review the source on /login which has a *decent* bit of intel there too ;)

### [TASK 12] - Question 2: Navigate to the directory you found in question one. What file stands out as being likely to contain sensitive data

This is the ever so clear flat-file database!

### [TASK 12] - Question 3: Use the supporting material to access the sensitive data. What is the password hash of the admin user

Download the database file, open a terminal and we can get to some cracking.

```bash
$sqlite3 <database-name>.db
sqlite3 > .tables
sessions users
sqlite3 > pragma table_info(users);
sqlite3 > select * from users;
```

After we review the table info, we are easily able to understand which column has our users md5 hash for the password.

### [Task 12] - Question 4: Crack the hash. What is the admin's plaintext password?

Grab the hash, throw it into the crackstation site. Inversely if you have hashcat installed we can do the following:

```bash
$hashcat –m 0 hashes /usr/share/wordlists/rockyou.txt
```

A solid breakdown of the above command can be found [at 4armed.com](https://www.4armed.com/blog/hashcat-crack-md5-hashes/).

Essentially the breakdown of our command is as follows:

|argument|description|
|---|---|
|-m 0|This will use md5 as the hash algorithm|
|hashes|The name of our file which contains the hashes we are looking to crack|
|/usr/share/wordlists/rockyou.txt|The wordlist which we are comparing our hashes against|

If you happened to NOT have your rockyou.txt in the above directory you can use the following find command to locate a few files which may work.

```bash
find . "rockyou" 2>/dev/null | grep "rockyou"
```