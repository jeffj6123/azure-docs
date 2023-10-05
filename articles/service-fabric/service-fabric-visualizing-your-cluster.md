---
title: Visualizing your cluster using Azure Service Fabric Explorer | Microsoft Docs
description: Service Fabric Explorer is an application for inspecting and managing cloud applications and nodes in a Microsoft Azure Service Fabric cluster.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''

ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2019
ms.author: mikhegn

---
# Visualize your cluster with Service Fabric Explorer

Service Fabric Explorer (SFX) is an open-source tool for inspecting and managing Azure Service Fabric clusters.

## Service Fabric Explorer download

Use the following links to download Service Fabric Explorer as a desktop application:

### Running Service Fabric Explorer from the cluster

Service Fabric Explorer is also hosted in a Service Fabric cluster's HTTP management endpoint. To launch SFX in a web browser, browse to the cluster's HTTP management endpoint from any browser - for example https:\//clusterFQDN:19080.

For developer workstation setup, you can launch Service Fabric Explorer on your local cluster by navigating to https://localhost:19080/Explorer. Look at this article to [prepare your development environment](service-fabric-get-started.md).

> [!NOTE]
> If your cluster is secured by a self-signed certificate you will receive an error message from the web browser "This site is not secure". You can simply proceed through most modern web browsers by overriding the warning. In a production environment your cluster should be secured using common name and a certificate authority issued certificate. 
>
>

## Connect to a Service Fabric cluster
To connect to a Service Fabric cluster, you need the clusters management endpoint (FQDN/IP) and the HTTP management endpoint port (19080 by default). For example https\://mysfcluster.westus.cloudapp.azure.com:19080. Use the "Connect to localhost" checkbox to connect to a local cluster on your workstation.

### Connect to a secure cluster
You can control client access to your Service Fabric cluster either with certificates or using Azure Active Directory (AAD).

If you attempt to connect to a secure cluster, then depending on the cluster's configuration you will be required to present a client certificate or sign in using AAD.

## Understand the Service Fabric Explorer layout
You can navigate through Service Fabric Explorer by using the tree on the left. At the root of the tree, the cluster dashboard provides an overview of your cluster, including a summary of application and node health.

![Service Fabric Explorer cluster dashboard][sfx-cluster-dashboard]

### View the cluster's layout
Nodes in a Service Fabric cluster are placed across a two-dimensional grid of fault domains and upgrade domains. This placement ensures that your applications remain available in the presence of hardware failures and application upgrades. You can view how the current cluster is laid out by using the cluster map.

You can filter by node name, node type, and health state.

![Service Fabric Explorer cluster map][sfx-cluster-map]

### View applications and services
The cluster contains two subtrees: one for applications and another for nodes.

You can use the application view to navigate through Service Fabric's logical hierarchy: applications, services, partitions, and replicas.

![Service Fabric Explorer application view][sfx-application-tree]

At each level of the tree, the main pane shows pertinent information about the item. For example, you can see the health status and version for a particular service.

![Service Fabric Explorer essentials pane][sfx-service-essentials]

### View the cluster's nodes
The node view shows the physical layout of the cluster. For a given node, you can inspect which applications have code deployed on that node. More specifically, you can see which replicas are currently running there.

## Actions
Service Fabric Explorer offers a quick way to invoke actions on nodes, applications, and services within your cluster.

For example, to restart a node, choose the node from the tree on the left, and then choose **Actions** > **Restart Node**.

![Deleting an application in Service Fabric Explorer][sfx-action]

> [!TIP]
> You can perform the same actions by clicking the ellipsis next to each element.
>
> Every action that can be performed through Service Fabric Explorer can also be performed through PowerShell or a REST API, to enable automation.
>
>

## Event Store
EventStore is a feature offered by the platform that provides Service Fabric platform events available in the Service Fabric Explorer and through REST API. You can see a snapshot view of what's going on in your cluster for each entity e.g. node, service, application and query based on the time of the event. You can also Read more about the EventStore at the [EventStore Overview](service-fabric-diagnostics-eventstore.md).   

Service Fabric Explorer provides two ways to interpret these events; the timeline and the list view. The timeline will usually be one of the first places used when investigating key events in a cluster. The timeline can be overlapped with some other entities(nodes and cluster).

For example if you see logging from your application that implies it restarted multiple times in an hour, you could use the application's event tab to see what the exit reason was and if there were any cluster upgrades in that time range. The event store viewer can provide a lot of information for initially investigating an issue in a cluster.

Some other high value events are :
  * Previous Cluster Upgrades
  * Node Down
  * Application Process Exits
  * Partition Primary Swaps

When is this tab most useful?
* Historical information

![EventStore][sfx-eventstore]

## Repair Jobs
![overview][sfx-repair-job-overview]
Repair jobs can be used to perform node operations like restart, usually requested by either the customer owner or another azure resource like VMSS. The repair job manager is enabled by default for silver reliability clusters and above. This tab can be very useful when investigating why a node or nodes went down recently. There are two different ways to look at all the jobs together by using the timeline viewer or the duration graph. The duration graph can be useful when needing to see if there are jobs that are outliers.

![duration graph][sfx-duration-graph]

> [!NOTE]
> You can filter and order the Pending and Completed Jobs tables and this will be reflected in the two graphs. This is a very easy way to only look for jobs affecting a particular node or timeframe.

When you expand a repair job you can see an overview of the job; a timeline showing time spent in each phase, executor job data which is included with most azure jobs and a full description for any other related information. If it is an active job you will also see all nodes impacted and targeted with their current status.

![repair job in progress][sfx-repair-job-in-progress]

When is this tab most useful?
* Stuck disabling nodes
* Stuck VMSS operations like restart or reimage
* Viewing recent repair jobs

>[!NOTE]
>As of Service Fabric version 6.4. the EventStore APIs are only available for Windows clusters running on Azure only. We are working on porting this functionality to Linux as well as our Standalone clusters.

## Image Store Viewer
Image store viewer is a feature offered if using Native Image Store that allows for viewing the current contents of the Image store and get file and folder information, along with removing files/folders.

![Service Fabric Explorer cluster map][sfx-imagestore]

When is this tab most useful?
* Missing Imagestore files
* Determining Imagestore content size
* Removing old Imagestore content


## Next steps
* [Managing your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Service Fabric application deployment using PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.gif
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-action]: ./media/service-fabric-visualizing-your-cluster/SfxAction.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
[sfx-eventstore]: ./media/service-fabric-diagnostics-eventstore/eventstore.png
[sfx-imagestore]: ./media/service-fabric-visualizing-your-cluster/SfxImageStore.png

[sfx-repair-job-in-progress]: ./media/service-fabric-visualizing-your-cluster/SfxRepairJobInProgress.png
[sfx-repair-job-overview]: ./media/service-fabric-visualizing-your-cluster/SfxRepairJobOverview.png
[sfx-duration-graph]: ./media/service-fabric-visualizing-your-cluster/SfxDurationGraph.png