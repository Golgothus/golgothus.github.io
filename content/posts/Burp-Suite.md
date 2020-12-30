---
layout: page
title: "Burp Suite 101"
permalink: /blog/Burp-Suite/
---

Burp Suite Intruder Documentation can be found [here](https://portswigger.net/burp/documentation/desktop/tools/intruder/)

Burp Suite Intruder Attack Type Documentation can be found [here](https://portswigger.net/burp/documentation/desktop/tools/intruder/positions)

There are four major types of attacks that can be used in Intruder:

1. Sniper
   1. The most popular attack type, this cycles through our selected positions, putting the next available payload (item from our wordlist) in each position in turn. This uses only one set of payloads (one wordlist).
2. Battering Ram
   1. Similar to Sniper, Battering Ram uses only one set of payloads. Unlike Sniper, Battering Ram puts every payload into every selected position. Think about how a battering ram makes contact across a large surface with a single surface, hence the name battering ram for this attack type.
3. Pitchfork
   1. The Pitchfork attack type allows us to use multiple payload sets (one per position selected) and iterate through both payload sets simultaneously. For example, if we selected two positions (say a username field and a password field), we can provide a username and password payload list. Intruder will then cycle through the combinations of usernames and passwords, resulting in a total number of combinations equalling the smallest payload set provided.
4. Cluster bomb
   1. The Cluster Bomb attack type allows us to use multiple payload sets (one per position selected) and iterate through all combinations of the payload lists we provide. For example, if we selected two positions (say a username field and a password field), we can provide a username and password payload list. Intruder will then cycle through the combinations of usernames and passwords, resulting in a total number of combinations equalling usernames x passwords. Do note, this can get pretty lengthy if you are using the community edition of Burp.

   Intruder Attack Type Selection

   For our purposes, we'll be returning to the SQL injection vulnerability we previously discovered through using Repeater.

[SQLi wordlist](https://raw.githubusercontent.com/fuzzdb-project/fuzzdb/master/attack/sql-injection/detect/xplatform.txt)

Now we are getting ready for the actual attack, I found it useful to pull the intruder attack configuration document from [here](https://portswigger.net/support/configuring-a-burp-intruder-attack)

## Task 9 Flag 8

Performed the following steps to perform my SQLi Attack:

1. Proxy tab > HTTP history > (Located the a POST attempt at /rest/user/login), hit ctrl + i / sent to intruder
2. From Intruder tab > Positions, I setup my attack to be as follows (make sure to disable URL-Encoding under payloads, and import your SQLi fuzz payload)

```Plaintext
POST /rest/user/login HTTP/1.1
Host: 10.10.8.150
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: application/json, text/plain, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://10.10.8.150/
Content-Type: application/json
Content-Length: 34
Connection: close
Cookie: io=<your value>; continueCode=<value>; cookieconsent_status=dismiss

{"email":"§test§","password":"test"}
```

## Task 10 Flag 3

> Make sure to look at your responses, and **NOT** requests!

## Task 10 Flag 6 / 7

Both of these flags are dumb...

## Task 11 - flags x

[Comparer Documentation](https://portswigger.net/burp/documentation/desktop/tools/comparer)
