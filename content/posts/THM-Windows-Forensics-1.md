---
title: "Windows Forensics 1"
date: 2022-11-14T19:07:15
# get-date -format yyyy-MM-ddTHH:mm:ss
draft: false
author: "Golgothus"
description: "THM - Intro to Windows Registry Forensics"
summary: "Exercise - Brief overfiew of Windows Registry"
tags: ["DFIR"]
featuredImage: "/images/city.jpg"
categories: ["Forensics"]
---

# Registry Key Info

Typical key structure for the registry is:

|Folder / predefined key|Description|
|---|---|
|HKey_CURRENT_USER| Contains root config regarding the user who is currently logged on. This directory can consist of folder, screen color and Control Panel settings. (HKCU)|
|HKEY_USERS| Contains all actively loaded user profiles|
|HKEY_LOCAL_MACHINE|Contains config information particular to the computer (for any user). (HKLM)|
|HKEY_CLASSES_ROOT|Subkey of `HKLM\Software`. Information stored here makes that the correct program association occurs when you open a file through Windows Explorer. Sometimes abbreviated as HKCR. `HKLM\Software\Classes` contains default settings for applications across the system. `HKCU\Software\Classes` contains config information specific to that users profile. If you are writing values to a key under HKCR, and the key has a   pre-existing value within `HKCU\Software\Classes`, the system will store the information there instead of under `HKEY_LOCAL_MACHINE\Software\Classes`.|
|HKEY_CURRENT_CONFIG| Contains information about the hardware profile that is used by the local computer at system startup.|