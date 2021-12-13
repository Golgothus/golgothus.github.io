---
title: "DPS Challenge"
date: 2020-12-25T01:02:30-06:00
draft: false
author: "Golgothus"
description: "My attempt at completing the TryHackMe.com DPS Challenge"
summary: "TryHackMe - DPS Challenge."
tags: ["TryHackMe","Wireshark","PCAP"]
featuredImage: "/images/japan.jpg"
categories: ["THM"]
---

We open the challenge and we're given a .pcapng file to review. Unfortunately I did not have Wireshark installed, but no problem, takes only but a few minutes go get setup.

Once I opened Wireshark, I like to start in the **Statistics > Endpoints** section. I open the **IPv4** column and see there is a good amount of communication going from **192.168.200.5**. I go ahead and set this as my filter. However, after reviewing the information here, it's way too much to try and work through for now.

So I went ahead and opened up **File > Export Objects > HTTP** quite a few exportable items, but nothing which seemed useful. Thankfully I ended up checking under **File > Export Objects > SMB** and found some items! After reviewing the exported items we see there is communication from:

`192.168.200.7 > 192.168.200.5` going over SMB. Seems pretty odd, especially since the file we found here was the missing PDF.

Let's go ahead and try to answer some of these questions!

## 1. What is the IP address of the hacker?
192.168.200.7

When reviewing the traffic from the SMB export, **192.168.200.7** was the IP of the machine communicating to Tom's system.

## 2. Where is Tom flying to?
Phoenix

When we actually go through the process of exporting the document *File > Export Objects > SMB*, we can export the flight.pdf file and see this information.

## 3. How much did Tom pay (in USD) in total for the flight? 
219.30

When we actually go through the process of exporting the document *File > Export Objects > SMB*, we can export the flight.pdf file and see this information.

## 4. What is the flag on Tom's computer? (format: @@@-@@@-####)
SKY-LEAK-2304

When we actually go through the process of exporting the document *File > Export Objects > SMB*, we can export the data.png file and see this information.

## 5. What is the name of the Linux distribution used by the attacker? 
Kali

This information is obtained from the SMB trace between Tom and our attacker. You can apply the following filter in Wireshark to see this communication:

```text
ip.src == 192.168.200.7 && ip.dst == 192.168.200.5 && smb2
```

## 6. Tom had an executable file in his data volume, what is the name of that file?
Wireshark-win64-3.0.6.exe

This information is obtained from the SMB trace between Tom and our attacker. You can apply the following filter in Wireshark to see this communication:

```text
ip.src == 192.168.200.7 && ip.dst == 192.168.200.5 && smb2
```

## 7. What is Tom's account password?
castlepark93

I needed some assistance with this one, and apparently you can locate all the information needed to crack the NTLM hash! Thanks to Birchyyyyy for the resource to follow on how to do exactly this.

[Information for NTLM hash cracking](https://research.801labs.org/cracking-an-ntlmv2-hash/)