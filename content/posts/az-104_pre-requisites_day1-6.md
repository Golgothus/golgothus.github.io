---
title: "AZ-104: Prerequisites for Azure administrators"
date: 2022-01-10T01:02:30-06:00
draft: false
author: "Golgothus"
description: "Microsoft AZ-104 Learning Path."
summary: "Exercise - Design and implement IP addressing for Azure virtual networks "
tags: ["Azure, AZCLI","Azure Powershell"]
featuredImage: "/images/city.jpg"
categories: ["Azure"]
---

Figured I'd do something somewhat close to a 100 days of code, but with some of the content and material I'm learning for Azure. Specifically in regards to the Azure 104 exam, Azure Administrator. I've already started to work through some of the modules and sections, but I will be posting some of the snippets and key takeaways I think might benefit myself, or any others who may end up stumbling across this resource.

Currently I am working on taking on ~1 module / ~1 hour, a day; starting on Monday and eneding on Friday. This way it will leave the weekend free for whatever shenanigans I might be up to. The first module I've finished is the [AZ-104: Prerequisites for Azure administrators](https://docs.microsoft.com/en-us/learn/certifications/azure-administrator/). I will be heavily focusing on the Azure Powershell portions of this entire learning path as that is what I am most familiar with.

This module covers the following sections:
- [Configure Azure resources with tools](https://docs.microsoft.com/en-us/learn/modules/configure-azure-resources-tools/?ns-enrollment-type=LearningPath&ns-enrollment-id=learn.az104-admin-prerequisites)
- [Use Azure Resource Manager](https://docs.microsoft.com/en-us/learn/modules/use-azure-resource-manager/)
- [Configure Resources with Azure Resource Manager Templates](https://docs.microsoft.com/en-us/learn/modules/configure-resources-arm-templates/)
- [Automate Azure Tasks Using Scripts with Powershell](https://docs.microsoft.com/en-us/learn/modules/automate-azure-tasks-with-powershell/)
- [Control Azure Services with the CLI](https://docs.microsoft.com/en-us/learn/modules/control-azure-services-with-cli/)
- [Deploy Azure Infrastructure by using JSON ARM Templates](https://docs.microsoft.com/en-us/learn/modules/create-azure-resource-manager-template-vs-code/)

If you are planning on doing any bit of automation with resource creation in Azure the following sections were pretty solid, they even allow you to use a sandbox so your Azure account won't be charged.
- [Automate Azure Tasks Using Scripts with Powershell](https://docs.microsoft.com/en-us/learn/modules/automate-azure-tasks-with-powershell/)
- [Deploy Azure Infrastructure by using JSON ARM Templates](https://docs.microsoft.com/en-us/learn/modules/create-azure-resource-manager-template-vs-code/)

A few of the very specific takeaways I've learned are:
- You will need the az module for Powershell if you want to do local changes to your Azure subscription(s)
  - Download the AZ powershell module by running `import-module -name az`
  - Additionally you will need to run the command `connect-azaccount` within Powershell, once your module is installed, this allows you to authenticate to Azure
  - If you're unable to run scripts, this may be due to the script execution policy enforced on your endpoint, this can be modified by running `Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope CurrentUser`

> At the time of writing this document, MSFT recommends using Powershell version 7.1.3 or higher. You can check your psh version by running `$psversiontable.psversion`

The following might help to verify if your module is installed properly:

```powershell
get-module
Get-Command -module az* | Select-Object -Property name
```

The initial results from `get-module` should provide a similar output to the one below where you see the names of Azure modules and a truncated list of commands for each module.

```text
ModuleType Version    PreRelease Name                                ExportedCommands
---------- -------    ---------- ----                                ----------------
Script     5.7.0                 az
Script     2.2.7                 Az.Accounts                         {Add-AzEnvironment, Clear-AzContext, Clear-AzDefault, Connect-AzAccount…}        
Script     1.1.1                 Az.Advisor                          {Disable-AzAdvisorRecommendation, Enable-AzAdvisorRecommendation, Get-AzAdvisor… 
Script     2.0.2                 Az.Aks                              {Disable-AzAksAddOn, Enable-AzAksAddOn, Get-AzAksCluster, Get-AzAksNodePool…}    
Script     1.1.4                 Az.AnalysisServices                 {Add-AzAnalysisServicesAccount, Export-AzAnalysisServicesInstanceLog, Get-AzAna… 
Script     2.2.0                 Az.ApiManagement                    {Add-AzApiManagementApiToGateway, Add-AzApiManagementApiToProduct, Add-AzApiMan… 
```

You may also run `get-command -module <specific module name> | select-object -property name` to find commands for the specific module. Example: `get-command -module az.aks | select-object -property name`.

## View resource group information (Powershell)

```Powershell
# view available subscriptions
get-azsubscription

# set variable for Azure Subscription ID to change Azure Context
$context = Get-AzSubscription -SubscriptionId <subscription ID>
Set-azcontext $context
# view which subscription you're working within
get-azcontext

# view available resource groups
get-azresourcegroup | format-table

# view available resources within a resource group
get-azresource -name <resourcegroupname> | format-table


new-azvm `
    -resourcegroupname <resource group name> `
    -Name <VM Name> `
    -Credential (get-credential) `
    -Location <location> `
    -image <image name>
```