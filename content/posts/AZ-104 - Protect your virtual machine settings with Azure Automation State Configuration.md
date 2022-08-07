---
title: "AZ-104 - Protect your virtual machine settings with Azure Automation State Configuration"
# get-date -format yyyy-MM-ddTHH:mm:ss
date: 2022-08-07T15:09:58
draft: false
author: "Golgothus"
description: "Microsoft AZ-104 Learning Path"
summary: "Azure 104 - Azure Administration learning path from https://docs.microsoft.com/en-us/learn/certifications/exams/az-104"
tags: ["Azure", "Virtual Machines","Compute"]
featuredImage: "/images/city.jpg"
categories: ["Azure"]
---

## What is Azure Automation State Configuration?

The use of scripting / automation to maintain a desired state configuration (DSC). The aim of this is to consistently deploy, reliably monitor, and automatically update the desired state of your resources.

Example:
- Updating operating system versions
- Updating services
- Installation and configuration of software and applications

### Why use Azure Automation State Configuration?

Azure Automation State Configuration uses Powershell DSC to automate the maintenance process. Azure ASC has a built-in pull server which can then target systems to pull pre-built configurations.

> Azure ASC is capable of targeting virtual or physical Windows / Linux machines in the cloud or on-prem.

Additionally, Azure ASC can be configured to send data to Azure Monitor logs to review the compliance of your nodes.

### What is Powershell DSC?

Powershell DSC is a declarative management platform that Azure ASC uses to configure, deploy, and control systems. It uses declarative statements (what you want to do) instead of execution (how you want to do it). We can define what our desired state _should_ look like, and Powershell DSC will take the necessary steps to match our definition.

> Powershell DSC is idempotent, this will have the same effect / result whether you run it once, or a several hundred times

Powershell DSC contains error handling and logic to meet your desired state, as compared to a case where you might create a simple one-liner to complete your desired task.

> With DSC, you describe the result rather than the process to achieve the result.

### What is the LCM?

The Local Configuration Manager (LCM) is a component of the Windows Management Framework (WMF). It is responsible for updating the state of a node to match the defined desired state.

When the LCM runs, it completes the following steps:

1. Get: Get the current state of the node.
2. Test: Compare the current state of a node against the desired state by using a compiled DSC script (.mof file).
3. Set: Update the node to match the desired state described in the .mof file.

> Side note, .MOF files can be used as means of persistence and should be monitored / kept secured https://docs.microsoft.com/en-us/powershell/dsc/pull-server/securemof?view=dsc-1.1

## Use Powershell DSC to achieve a desired state

Main Powershell cmdlet for DSC resource management is `Get-DSCResource`. The blow line will provide detailed columns for each section:

```powershell
Get-DscResource | select Name,Module,Properties
```

**Resource**|**Description**
:-----:|:-----:
File|Manages files and folders on a node
Archive|Decompresses an archive in the .zip format
Environment|Manages system environment variables
Log|Writes a message in the DSC event log
Package|Installs or removes a package
Registry|Manages a node's registry key (except HKEY Users)
Script|Executes PowerShell commands on a node
Service|Manages Windows services
User|Manages local users on a node
WindowsFeature|Adds or removes a role or feature on a node
WindowsOptionalFeature|Adds or removes an optional role or feature on a node
WindowsProcess|Manages a Windows process

Example of a DSC code block:

```powershell
Configuration MyDscConfiguration {              ##1
    Node "localhost" {                          ##2
        WindowsFeature MyFeatureInstance {      ##3
            Ensure = 'Present'
            Name = 'Web-Server'
        }
    }
}
MyDscConfiguration -OutputPath C:\temp\         ##4
```

> When you run a configuration block, it's compiled into a Managed Object Format (MOF) document. MOF is a compiled language created by Desktop Management Task Force, and it's based on interface definition language.

### Secure credentials in a DSC script

Use a pscredential object. Before running the script, get the credentials for the user, use the credentials to create a new PSCredential object, and pass this object into the script as a parameter.

Credentials aren't encrypted in .mof files by default. They're exposed as plaintext. To encrypt credentials, use a certificate in your configuration data. The certificate's private key needs to be on the node on which you want to apply the configuration. Certificates are configured through the node's LCM.

> Starting in PowerShell 5.1, .mof files on the node are encrypted at rest. In transit, all credentials are encrypted through WinRM.

## References:

https://docs.microsoft.com/en-us/learn/modules/protect-vm-settings-with-dsc/