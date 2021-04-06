---
title: "RangeForce - Windows Event Logging"
date: 2020-12-25T00:25:54-06:00
draft: false
author: "Golgothus"
summary: "Very long module on querying XML event logs and filtering them with XPath."
categories: ["Linux Fundamentals"]
tags: ["RangeForce","Powershell","Logging","Blue Team"]
featuredImage: "/images/city.png"
---
# RangeForce - Windows Event Logging

```Powershell
Get-WinEvent -Path C:\Users\ContosoAdmin\Desktop\SystemLog.evtx -FilterXPath 'Event/System/TimeCreated[@SystemTime="2019-12-13T08:24:27.5440626"]'
Get-WinEvent -Path C:\Users\ContosoAdmin\Desktop\SecurityLog.evtx -FilterXPath '*/*/TimeCreated[(@SystemTime>"2020-02-26T00:00:00Z") and (@SystemTime<"2020-02-26T23:59:00Z")] and */*/EventID=4624 and */*/Data[@Name="TargetUserName"]="Eleanor"'
wevtutil.exe qe /lf "c:\Users\ContosoAdmin\Desktop\SecurityLog.evtx" /q:"*/*/TimeCreated[(@SystemTime>'2020-02-26T00:00:00Z') and (@SystemTime<'2020-02-26T23:59:00Z')] /c:1"
wevtutil.exe qe /lf "c:\Users\ContosoAdmin\Desktop\SecurityLog.evtx" /q:"Event/EventData/Data[@Name='TargetUserName']='Eleanor' and */*/EventID=4624" /c:1 /rd
wevtutil.exe qe /lf "%userprofile%/dedsktop/SecurityLog.evtx" /q:"*/*/TimeCreated[(@SystemTime>'2020-02-26T00:00:00Z') and (@SystemTime<'2020-02-26T23:59:00Z')] and */*/EventID=4624 and */*/Data[@Name='TargetUserName']='Eleanor'"
Get-WinEvent -path C:\Users\ContosoAdmin\Desktop\ApplicationLog.evtx -FilterXPath "*/*/TimeCreated[@SystemTime>'2020-02-11T10:50:00:00Z' and (@SystemTime<'2020-02-11T11:10:00Z')] and */*/Provider[@Name='MsiInstaller']"
Get-WinEvent -path C:\Users\ContosoAdmin\Desktop\Systemlog.evtx -FilterXPath "*/*/TimeCreated[@SystemTime>'2020-02-03T00:00:00:00Z' and (@SystemTime<'2020-02-03T23:59:00Z')] and */*/EventID=13"
Get-WinEvent -path C:\Users\ContosoAdmin\Desktop\Securitylog.evtx -FilterXPath "*/*/TimeCreated[@SystemTime>'2020-02-26T00:00:00:00Z' and (@SystemTime<'2020-02-26T23:59:00Z')] and */*/EventID=4616 and */*/Data[@Name='SubjectDomainName']='NT Authority'"
$test = Get-WinEvent -path C:\Users\ContosoAdmin\Desktop\Securitylog.evtx -FilterXPath "*/*/TimeCreated[@SystemTime>'2020-02-26T00:00:00:00Z' and (@SystemTime<'2020-02-26T23:59:00Z')] and */*/EventID=4624 and */*/Data[@Name='TargetUserName']='Network Service'"
```