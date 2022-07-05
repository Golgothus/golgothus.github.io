---
title: "AZ-104 - Configure Azure Kubernetes Service"
# get-date -format yyyy-MM-ddTHH:mm:ss
date: 2022-07-05T12:32:43
draft: false
author: "Golgothus"
description: "Microsoft AZ-104 Learning Path"
summary: "Azure 104 - Azure Administration learning path from https://docs.microsoft.com/en-us/learn/certifications/exams/az-104"
tags: ["Azure", "Kubernetes","AKS"]
featuredImage: "/images/city.jpg"
categories: ["Azure"]
---

## Explore the AKS Terminology
**Pools** are groups of nodes with identical configurations.

**Nodes** are individual virtual machines running containerized applications.

**Pods** are a single instance of an application. A pod can contain multiple containers.

**Container** is a lightweight and portable executable image that contains software and all of its dependencies.

**Deployment** has one or more identical pods managed by Kubernetes.

**Manifest** is the YAML file describing a deployment.

## Explore the AKS cluster and node architecture
![Azure Kubernetes Network Diagram](../_resources/e49796a80738ad5d6e75f2f475b746d5-1.png)

### Nodes and Nodes Pools

To run your applications and supporting services, you need a Kubernetes node. An AKS cluster contains one or more nodes (Azure Virtual Machines) that run the Kubernetes node components and the container runtime.

- The kubelet is the Kubernetes agent that processes the orchestration requests from the Azure-managed node, and scheduling of running the requested containers.
- Virtual networking is handled by the kube-proxy on each node. The proxy routes network traffic and manages IP addressing for services and pods.
- The container runtime is the component that allows containerized applications to run and interact with additional resources such as the virtual network and storage. AKS clusters using Kubernetes version 1.19 node pools and greater use containerd as its container runtime. AKS clusters using Kubernetes prior to v1.19 for node pools use Moby (upstream docker) as its container runtime.

## Configure AKS Networking

### Services
Logically grouped set of pods which provide newtork connectivity

- **Cluster IP** - Creates an internal IP Address for use within the AKS cluster. Good for *internal-only* applications that support other workloads within the cluster.
- **NodePort** - Creatse a port mapping on the underlying node that allows the application to be accessed directly with the node IP address and port.
- **LoadBalancer** - Creates an Azure load balancer resource, configures an external IP Address, and connects the requested pods to the load balancer backend pool. To allow customers traffic to reach the application, load-balancing rules are created on the desired ports.
- **ExternalName** - Creats a specific DNS entry for easier application access

### Pods
Represents a single instance of your application. There are instances where a pod might contain multiple containers. These multi-container pods are scheduled togehter on the same node, and allow containers to share related resources.

> Best practice is to include resource limits for all pods to help the K8s scheduler understand what resources are needed and permitted

## Configure AKS Storage

![d0f0ea34cf708fe50e03819d7ff04689.png](../_resources/d0f0ea34cf708fe50e03819d7ff04689-1.png)

### Volumes
A way to store, retrieve, and persist data across pods and through the application lifecycle.

Traditional volumes used to store AKS data are created as Kubernetes resources backed by Azure Storage. These volumes can use Azure Disks or Azure Files.

- Azure Disks - Mounted as *ReadWriteOnce*, so they are only available to a single node
- Azure Files - can be used to mount an SMB Share backed by an Azure Storage account to pods. This allows you to share data across multiple nodes and pods.

### Persistent Volumes
Volumes are defined and created as part of the pod lifecycle and only exist until the pod is deleted. A persistent volume exists beyond the lifetime of an individual pod.

### Storage Classes
StorageClass defines the different tiers of storage i.e. Premiumd, Standard. The StorageClass also defines the reclaimPolicy, which controls the behavior of the underlying Azure storage resource when the pod is deleted.

In AKS, four initial StorageClasses are created for cluster using the in-tree storage plugins:

- default - uses Azure StandardSSD storage to createa a Managed Disk, the reclaimPolicy ensures the Azure Disk is deleted when the persistent volume that used it is deleted
- managed-premium - uses Azure Premium storage to create a Managed Disk, the reclaimPolicy ensures the Azure Disk is deleted when the persistent volume that used it is deleted
- azurefile - Uses Azure Standard storage to create an Azure File Share, reclaimPolicy dictates the underlying share is deleted when the persistent volume that used it is deleted
- azurefile-premium - Uses Azure Premium storage to create an Azure FIle Share, the reclaimPolicy ensures that the underlying File Share is deleted when the persistent volume that used it is deleted

If no StorageClass is used _default_ will be used. An additional StorageClass may be created through the use of _kubectl_.

## Configure AKS scaling to Azure Container Instances

![483c1964fd1519f16e585396a585053a.png](../_resources/483c1964fd1519f16e585396a585053a-1.png)

To further enable raplidly scaling your AKS clusters, you can integrate with Azure Containers Instances. This will assist in deploying additional compute nodes / resources to allow for more pods to be created in the event there are no existing  compute resources within the node pool. If ACI is enabled, this will trigger the cluster autoscaler to deploy additional nodes in the node pool.
