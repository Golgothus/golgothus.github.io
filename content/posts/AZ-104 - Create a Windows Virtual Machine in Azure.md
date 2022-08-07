---
title: "AZ-104 - Create a Windows virtual machine in Azure"
# get-date -format yyyy-MM-ddTHH:mm:ss
date: 2022-07-09T17:20:44
draft: false
author: "Golgothus"
description: "Microsoft AZ-104 Learning Path"
summary: "Azure 104 - Azure Administration learning path from https://docs.microsoft.com/en-us/learn/certifications/exams/az-104"
tags: ["Azure", "Virtual Machines"]
featuredImage: "/images/city.jpg"
categories: ["Azure"]
---

## Create a Windows virtual machine in Azure

### Sizing your VM

The size of a VM takes into consideration the following factors:
- memory
- CPU
- max storage capacity

VMs may be scaled up or down after their creation, but must be powered down first in order to complete this task. Below is a list of recommended sizes based on the purpose of the VM.

**What are you doing?**|**Consider these sizes**
:-----:|:-----:
General use computing / web Testing and development, small to medium databases, or low to medium traffic web servers|B, Dsv3, Dv3, DSv2, Dv2
Heavy computational tasks Medium traffic web servers, network appliances, batch processes, and application servers|Fsv2, Fs, F
Large memory usage Relational database servers, medium to large caches, and in-memory analytics.|Esv3, Ev3, M, GS, G, DSv2, Dv2
Data storage and processing Big Data, SQL, and NoSQL databases, which need high disk throughput and IO|Ls
Heavy graphics rendering or video editing, as well as model training and inferencing (ND) with deep learning|NV, NC, NCv2, NCv3, ND
High-performance computing (HPC) If you need the fastest and most powerful CPU virtual machines with optional high-throughput network interfaces|H

### Choosing storage options

Choose between HDD / SSD. Additionally there is an option to choose between standard tier and premium tier SSD. Premium SSD disks may be used for I/O intensive workloads, or mission critical systems needing to process data quickly.

### Mapping storage to disks

By default, two VHDs (Virtual Hard Disks) are provisioned for each VM:
- Operating System Disk (primary C: drive), max capacity of 2048 GB (2TB)
- Temporary Disk (configured as the D: drive), this will house any temporary storage for the OS or any apps and is sized based on the VM size
  - The Temporary Disk may also house the Windows paging file due to the drives usage
  - The Temporary Disk is not persistent, do not write data to the disk which you are willing to lose at any time

#### What about data?

Data may be stored on the C: drive, but a better approach is to create dedicated *data disks*. Each data disk can hold up to 32,767 gibibytes (GiB) of data, the maximum amount of storage is determined by the VM size you select

### Unmanaged vs. Managed disks

Just use Managed disks. Less hassle and is recommended.

> Unamanaged disks are limited to 20,000 I/O operations a second and will require multiple storage accounts should the opeation need to scale out past 40 VHDs running at max operation

### Network Communication

#### Planning your network

> It is better to create your network requirements _up front_ for all of your architecture and create the VNet structure you'll need separately, and then create your VMs and place them in the alread-created VNets

## Configure Azure Virtual Machine Network Settings
### Open ports in Azure VMs

By default, new VMs are locked down.

Apps can make outbound requests, but inbound requests are blocked by default. The only inbound communications allowed are from the same virtual newtork (other resources on the same local network), and from the Azure Load Balancer.

> When creating a new VM, we can allow the ability to use RDP, HTTP, HTTPS, and SSH; For additional access we will need to modify the firewall rules ourselves

The Process to allow additional inbound rules:
- Createa an NSG (Network Security Group)
- Create an inbound rule allowing traffic on the necessary ports (port 20 and 21 for active FTP support)

### Security Group Rules

![Azure NSG(Network Security Group) Diagram. Displaying NSG's for the subnet as well as an NSG for the associated NIC(Network interface card) for each VM. Image is the property of the Microsoft Learn website.](../_resources/2022-07-09_18_16_44-Window.png)

#### How Azure uses Network rules

Inbound traffic:
- First the traffic is processed by the subnet nsg
- Second the traffic is processed by the NIC nsg

Outbound traffic (inverse of inbound):
- First traffic is processed by the NIC nsg
- Second the traffic is processed by the subnet NSG

> Keep in mind that security groups are optional at both levels. If no security group is applied, then all traffic is allowed by Azure. If the VM has a public IP, this could be a serious risk, particularly if the OS doesn't provide some sort of firewall.

Rules are evaluated in a _priority_ order. Starting the the **lowest priority** rule. Deny rules always **stop** the evaluation.

In order for traffic to succeed it must pass through _all_ applied groups.

The last rule is always a **Deny All** rule. This is default to every NSG for both inbound and outbound traffic with a priority of 65500. For traffic to pass through the NSG, _you must have an allow rule_ or it will be blocked by the default final rule.
