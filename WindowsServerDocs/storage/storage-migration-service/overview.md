---
Title: Storage Migration Service overview
description: Brief description of topic for search engine results
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 09/24/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
---

# Storage Migration Service overview

Storage Migration Service makes it easier to migrate servers to a newer version of Windows Server. It provides a graphical tool that inventories data on servers and then transfers the data and configuration to newer servers—all without apps or users having to change anything.

This topic discusses why you'd want to use Storage Migration Service, how the migration process works, and what the requirements are for source and destination servers.

## Why use Storage Migration Service

Use Storage Migration Service because you've got a server (or a lot of servers) that you want to migrate to newer hardware or virtual machines. Storage Migration Service is designed to help by doing the following:

- Inventory multiple servers and their data
- Rapidly transfer files, file shares, and security configuration from the source servers
- Optionally take over the identity of the source servers (also known as cutting over) so that users and apps don't have to change anything to access existing data
- Manage one or multiple migrations from the Windows Admin Center user interface

![Diagram showing Storage Migration Service migrating files & configuration from source servers to destination servers, Azure VMs, or Azure File Sync.](media\overview\storage-migration-service-diagram.png)

**Figure 1: Storage Migration Service sources and destinations**

## How the migration process works

Migration is a three-step process:

1. **Inventory servers** to gather info about their files and configuration (shown in Figure 2).
2. **Transfer (copy) data** from the source servers to the destination servers.
3. **Cut over to the new servers** (optional).<br>The destination servers assume the source servers' former identities so that apps and users don't have to change anything. <br>The source servers enter a maintenance state where they still contain the same files they always have (we never remove files from the source servers) but are unavailable to users and apps. You can then decommission the servers at your convenience.

![Screenshot showing a server ready to be scanned](media/migrate/inventory.png)
**Figure 2: Storage Migration Service inventorying servers**

## Requirements

To use Storage Migration Service, you need the following:

- A **source server** to migrate files and data from
- A **destination server** running Windows Server 2019 to migrate to—Windows Server 2016 and Windows Server 2012 R2 work as well but are around 50% slower
- An **orchestrator server** running Windows Server 2019 to manage the migration  <br>If you're migrating only a few servers and one of the servers is running Windows Server 2019, you can use that as the orchestrator. If you're migrating more servers, we recommend using a separate orchestrator server.
- A **PC or server running [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md)** to run the Storage Migration Service user interface, unless you prefer using PowerShell to manage the migration. The Windows Admin Center and Windows Server 2019 version must both be at least version 1809. 

### Security requirements

- A migration account that is an administrator on the source computers.
- A migration account that is an administrator on the destination computers.
- The orchestrator computer must have the File and Printer Sharing (SMB-In) firewall rule enabled *inbound*.
- The source and destination computers must have the following firewall rules enabled *inbound* (though you might already have them enabled):
  - File and Printer Sharing (SMB-In)
  - Netlogon Service (NP-In)
  - Windows Management Instrumentation (DCOM-In)
  - Windows Management Instrumentation (WMI-In)
  
  > [!TIP]
  > Installing the Storage Migration Service Proxy service on a Windows Server 2019 computer automatically opens the necessary firewall ports on that computer.
- If the computers belong to an Active Directory Domain Services domain, they should all belong to the same forest. The destination server must also be in the same domain as the source server if you want to transfer the source's domain name to the destination when cutting over. Cutover technically works across domains, but the fully-qualified domain name of the destination will be different from the source...

### Requirements for source servers

The source server must run one of the following operating systems:

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows Server 2003 R2
- Windows Server 2003

### Requirements for destination servers

The destination server must run one of the following operating systems:

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2

> [!TIP]
> Destination servers running Windows Server 2019 have double the transfer performance of earlier versions of Windows Server. This performance boost is due to the inclusion of a built-in Storage Migration Service proxy service, which also opens the necessary firewall ports if they're not already open.

## See also

- [Migrate a file server by using Storage Migration Service](migrate-data.md)
- [Storage Migration Services frequently asked questions (FAQ)](faq.md)
- [Storage Migration Service known issues](known-issues.md)