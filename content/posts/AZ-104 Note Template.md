---
title: "AZ-104 Notes"
date: 2020-12-25T01:02:30-06:00
# get-date -format yyyy-MM-ddTHH:mm:ss
draft: true
author: "Golgothus"
description: "Microsoft AZ-104 Cloud Challenge Notes"
summary: "Exercise - Design and implement IP addressing for Azure virtual networks "
tags: ["Azure", "AZCLI"]
featuredImage: "/images/city.jpg"
categories: ["Azure"]
---

#  Control access to Azure Storage with shared access signatures

## Authorization options for Azure Storage

### Public access

Azure Blobs / Containers

There are two separate settings that affect public access:

The Storage Account. Configure the storage account to allow public access by setting the AllowBlobPublicAccess property. When set to true, Blob data is available for public access only if the container's public access setting is also set.

The Container. You can enable anonymous access only if anonymous access has been allowed for the storage account. A container has two possible settings public access: Public read access for blobs, or public read access for a container and its blobs. Anonymous access is controlled at the container level, not for individual blobs. This means that if you want to secure some of the files, you need to put them in a separate container that does not permit public read access.

### Azure Active Directory

Use the Azure AD option to securely access Azure Storage without storing any credentials in your code. AD authorization takes a two-step approach. First, you authenticate a security principal that returns an OAuth 2.0 token if successful. This token is then passed to Azure Storage to enable authorization to the requested resource.

Use this form of authentication if you're running an app with managed identities or using security principals.

## Configure Azure App Service

### Secure an App Service

Logging and tracing
If you enable application logging, you will see authentication and authorization traces directly in your log files. If you see an authentication error that you didn’t expect, you can conveniently find all the details by looking in your existing application logs. If you enable failed request tracing, you can see exactly what role the authentication and authorization module may have played in a failed request. In the trace logs, look for references to a module named EasyAuthModule_32/64.

### Back up an app service

#### Considerations

- You need an Azure storage account and container in the same subscription as the app that you want to back up. After you have made one or more backups for your app, the backups are visible on the Containers page of your storage account, and your app. In the storage account, each backup consists of a.zip file that contains the backup data and an .xml file that contains a manifest of the .zip file contents. You can unzip and browse these files if you want to access your backups without actually performing an app restore.
- Backups can be up to 10 GB of app and database content.

ps auxxwm | grep 'Z'