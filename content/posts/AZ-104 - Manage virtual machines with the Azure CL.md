---
title: "AZ-104 - Manage virtual machines with the Azure CLI "
# get-date -format yyyy-MM-ddTHH:mm:ss
date: 2022-07-05T16:29:04
draft: false
author: "Golgothus"
description: "Microsoft AZ-104 Learning Path"
summary: "Azure 104 - Azure Administration learning path from https://docs.microsoft.com/en-us/learn/certifications/exams/az-104"
tags: ["Azure", "AZCLI"]
featuredImage: "/images/city.jpg"
categories: ["Azure"]
---

## Exercise - Create a virtual machine
`az vm create`, this will create a VM in a resource group. There are four parameters which _must_ be supplied:

- --resource-group: resource group that will own the machine
- --name: name of the vm, must be unique within the _resource group_
- --image: OS for the vm
- --location: region which to place the VM

> Useful to add `--verbose` to see progress while the VM is being created. To automatically return to the commandline use `--no-wait`

```AzCLI
az vm create \
  --resource-group learn-4f4d525c-6fad-4b50-99cb-7606d6c57647 \
  --location westus \
  --name SampleVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys \
  --verbose
```

> To find help for a specific command, use `--help`.

`az vm --help` will provide a list of commands and _subcommands which can be utilized with `az vm`.

## Exercise - Explore other VM images

The following will provide a list of offline images: `az vm image list --output table` will produce a list of available VM images.

We can additionally use `az vm image list --output table --all` to provide an up-to-date list. There are filters we can use when viewing this list to try and condense it to certain images we are interested in using.

Available filters:
- --publisher
- --sku
- --offer

## Exercise - Query system and runtime information about the VM

Get the list of existing VMs regardless of resource group:
- `az vm list`

Format the results from a command:
- `az vm list --output table`, default format is json
- additional options are jsonc (colorized JSON) and tsv (tab seperated values)

Get a list of ip addresses for operational VMs
- `az vm list-ip-addresses --output table`

### Add filters to queries with JMESPath

Using JMESPath is a means to filter JSON queries and receive specific data you're looking for, based off an array.

### Filters you Azure CLI queries

```AZCLI
az vm show \
    --resource-group learn-4f4d525c-6fad-4b50-99cb-7606d6c57647 \
    --name SampleVM \
    --query "osProfile.adminUsername"
```

> The query values are case-sensitive

The following will pull the size assigned to the specific VM.

```AZCLI
$ az vm show \
> -g $rgroup \
> -n testVM \
> --query hardwareProfile.vmSize
"Standard_DS1_v2"
```

## Exercise - Start and stop your VM with the Azure CLI

### Stop a VM

```azcli
az vm stop --name "VMName" --resource-group "resource_group"
```

Get the power state of the VM:

```azcli
az vm get-instance-view \
    --name SampleVM \
    --resource-group learn-8a9e674d-1e48-4a9b-9200-e6e3fd49d17c \
    --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" -o tsv
```

## Resources for review

- https://docs.microsoft.com/en-us/cli/azure/
- https://docs.microsoft.com/en-us/cli/azure/reference-index