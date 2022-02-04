---
title: "Sysmon"
# get-date -format yyyy-MM-ddTHH:mm:ss
date: 2022-01-21T17:22:09
draft: false
author: "Golgothus"
description: "TryHackMe Sysmon Room"
summary: "Exercise - Install and configure sysmon for 'idea' threat hunting and investigations."
tags: ["TryHackMe","Blue Team"]
featuredImage: "/images/city.jpg"
categories: ["TryHackMe"]
---

Recommended Sysmon EventIDs worth monitoring:
- Event ID 1: Process Creation
- Event ID 3: Network Connection
- Event ID 7: Image Loaded
- Event ID 8: CreateRemoteThread
- Event ID 11: File Created
- Event ID 12 / 13 / 14: Registry Event
- Event ID 15: FileCreateStreamHash
- Event ID 22: DNS Event

Filtering syntax:
- Filter by Event ID: `*/System/EventID=<ID>`
- Filter by XML Attribute/Name: `*/EventData/Data[@Name="<XML Attribute/Name>"]`
- Filter by Event Data: `*/EventData/Data=<Data>`