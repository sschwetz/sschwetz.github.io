---
#**************************************
lang: en-AU
layout: page
social-share: true
comments: false

typora-copy-images-to: ../../assets/img/optus-handbook
typora-root-url: ../../
#**************************************

#*************************************
#fill this if you have renamed the page
redirect_from:
#*************************************

title: Optus Survival Guide
subtitle: OSS Cloud Version 2.0
author: Stephen Schwetz
updated: 
cover-img: 
thumbnail-img: 
full-width: false

share-title: 
share-description: 
share-img: 

tags:
  - Optus 
  - Handbook
  - OSS
  - Cloud

toc: false
---

### Revision History

| **Version** | **Date**   | **Modified By** | **Description**                                              |
| ----------- | ---------- | --------------- | ------------------------------------------------------------ |
| 1.0         |            | David Nouwans   | Initial Release                                              |
| 1.1         | 15/12/2021 | Stephen Schwetz | Added Config for additional compute switch at BT and RR      |
| 1.2         | 24/03/2022 | Stephen Schwetz | Updated Templated to utilise SVT post NSX implementation     |
| 1.3         | 05/03/2022 | Stephen Schwetz | Migrated to Apple Notes and split documents into new multi-part layout |
| 1.4         | 06/07/2022 | Stephen Schwetz | Recombined into Typora Markdown Applied new Documentation styles |
| 2.0-draft1  | 07/09/2023 | Stephen Schwetz | Near complete rewrite for DHCI                               |

---

# Handy Things to Know

Below are some things that make you seem brighter to your peers.

| Jargon          | Unjargon                                                     |
| :-------------- | ------------------------------------------------------------ |
| IRF             | HPE **I**ntelligent **R**esilient **F**ramework              |
| OSS             | Optus **O**perational **S**upport **S**ystems                |
| NPTE            | **N**on-**P**roduction **T**est **B**ed - also referred to as Lab |
| VRF             | **V**irtual **R**outing and **F**orwarding                   |
| Stretched VLAN  | Layer 2 networking tunnelled across IP, usually using VXLAN or GENEVE |
| VXLAN           | **V**irtual E**x**tensible **LAN** - a network virtualisation technology |
| N-VDS           | **N**SX **V**irtual **D**istributed **S**witch. This is set up at the hypervisor level and maintained in the NSX Manager (no longer recommended) |
| VDS             | **V**irtual **D**istributed **S**witch This is set at the vCenter  level |
| Edge Node       | All traffic into and out of NSX-T has to traverse an edge node. Responsible for routing, firewall, and L7 apps e.g. load balancing |
| The NSX Manager | All routing and logical switching changes are implemented within the NSX Manager GUI. |
| Tier-0 Gateway  | NSX-T gateway that is used for Traffic traversing North-South. Responsible for Firewall, Dynamic |
| Tier-1 Gateway  | An edge node cluster is a logical group of NSX-T virtual nodes that host the Tier-0 logical router functions. All workload data in and out of the NSX-T virtual environment must traverse an edge node |
| Clustering      | The act of running more than 1 Server to perform HA          |
| ECMP            | Equal Cost Multi-Path is used to perform load-sharing across multiple peers |
| BFD             | **B**i-**F**orward **D**etection is used to detect a failure where two interfaces are not directly connected, <br /><br />e.g.  [router_a] <=\=>[switch] <\==>[router_b] has a link failure [router_a] <=\=>[switch]<\=XXX=>[router_b]<br /><br />Normally this would take 180 seconds for the link to fail over on router_a as its physical interface is up. BFD will failover this link much quicker |
| HA              | High Availability                                            |
| Affinity Rules  | Rules that are applied to hosts and guests to make them run together on the host/separate hosts/or pinned to a host |
| Pinning VM      | Using affinity rules to make a VM only run on a specific host |
| Standard Switch | The original ESX virtualised switch. Configured at the hypervisor level |
| GENEVE          | **GE**neric **N**etwork **V**irtualization **E**ncapsulation |
| dHCI            | **d**isaggregated **H**yper-**C**onverged **I**nfrastructure |
| SVT             | HPE SimpliVity                                               |
| ISCSI           | **I**nternet **S**mall **C**omputer **S**ystem **I**nterface - works on top of the Transport Control Protocol (TCP) and allows the SCSI command to be sent end-to-end over local-area networks (LANs), wide-area networks (WANs) or the internet. |

---

# Background

Optus Fixed Infrastructure Pty Limited (Optus) previously engaged Hewlett Packard Enterprise (HPE) to design and deploy IT infrastructure required for new private cloud platform to host operational support systems (OSS) applications. This was completed based on HPE SimpliVity compute and storage products.

Optus has engaged HPE to provide additional compute and storage infrastructure for the OSS Cloud. This will be done using HPE Nimble Storage dHCI infrastructure, HPE FlexFabric networking, and HPE StoreOnce backup devices. The new dHCI infrastructure will replace the aging SimpliVity infrastructure.

This document is intended to be used as a reference for understanding and supportint of the netwoeking components of the new private cloud platform.

# Accessing Optus Environment.

The access to Optus' environment is slightly different to other customers. Optus has two virtual Remote Desktop Servers that are explicitly permitted for Access to Optus' Citrix Farm:

# Connecting to the CSN

The CSN is ITOC's Secure network. From this network, you can access the ITOC toolsets and remote access into the customer's environment. The CSN is air-gapped from all HPE and external ingress, and to access it, you need to install the [Fortinet Forticlent VPN Client](https://forticlient.coim).

## CSN VPN Endpoints

CSN has 2 POPs that will permit access via the Forticlient

| FQDN              | Description                |
| ----------------- | -------------------------- |
| kremote.onhpe.net | Newcastle VPN Concentrator |
| cremote.onhpe.net | Canberra VPN Concentrator  |

### Set up the VPN Client

1. Click on `create connection`, or if you already have connections, click on the hamburger icon and select `new connection`.

1. Fill out the required setup details on the create connection screen.

   ![image-20230914051033061](/assets/img/optus-handbook/image-20230914051033061.png)

1. Click Save. This will then return you to the connection screen

---

### Connecting to the VPN Client

1. Fill out the Username and Password field.  If you have a yubikey you can supply your 2FA one-time password by putting your password in and then pressing <kbd>,</kbd> and then supplying the one-time password.

![image-20230912103453510](/assets/img/optus-handbook/image-20230912103453510.png)

2. Click Connect, and the VPN will connect.

![image-20230912110727104](/assets/img/optus-handbook/image-20230912110727104.png)

---

### CSN Jump Hosts

Optus has dedicated Jumphosts, to connect to these it is recommended to use a client like Remote Desktop Manager Free, as this will allow you to type in passwords, as Optus has blocked copy and  paste  into the VDI.
| IP           | Name                   | Description             |
| ------------ | ---------------------- | ----------------------- |
| 172.25.167.8 | KINFVWOPT001.onhpe.net | Kotara Optus Jumphost   |
| 172.25.67.8  | CINFVWOPT001.onhpe.net | Canberra Optus Jumphost |

## Connecting to the Optus VDI


Once connected to the environment, you must open a web session to their Citrix vDS Farm server: 

* [https://hps.ayrowa.au.singtelgroup.net/vpn/index.html](https://hps.ayrowa.au.singtelgroup.net/vpn/index.html)

## File Transfers

The jump hosts below are not CSN-joined; each has their AD Instance. the external clipboard has been turned off on all VDS accounts; you cannot copy and paste into and out of the Optus secure environment. If you want to move files into and out of the environment, you can use either:

* HPE CSN-based git - [https://git.onhpe.net](https://git.onhpe.net) (suitable for small files only)
* HPRC  - [https://hprc-ui.its.hpecorp.net/jetspeed/portal/hprc](https://hprc-ui.its.hpecorp.net/jetspeed/portal/hprc) (you must be on HPE Corp VPN to create the user account). This site will offer you a dropbox of up 20GB and more if justifiable.)

There is no direct access to the OSS Cloud except to the HPE managed Windows SPOPs

## Optus SPOPs

The following SPOPS can be accessed from the AYROWA HPE VDI

| IP             | Name       | Description     | AD Domain           |
| -------------- | ---------- | --------------- | ------------------- |
| 172.28.22.73   | 22rrvhpe01 | Production SPOP | hpe.ossmpc.net      |
| 172.19.175.110 | o3ebvhpe01 | Production SPOP | hpe.ossmcp.net      |
| 10.201.161.22  | o2rovhpe01 | NPTE SPOP       | npte.hpe.ossmpc.net |

# Exchange Locations

|   Exchange   |                      Addreess                       |                        Rack Locations                        |
| :----------: | :-------------------------------------------------: | :----------------------------------------------------------: |
|  Blacktown   |  25-26 Chicago Ave <br />Blacktown <br />NSW 2148   | 22BT.G0ZK.F10 (SVT) <br />22BT.G0ZK.F12 (SVT) <br />22BT.G0ZJ.F04 (dHCI) |
|    Mascot    |     9 Canal Road <br />St Peters <br />NSW 2044     | 22RR.G0EA.F03 (SVT)<br/>22RR.G0ME.F01 (SVT)<br/>22RR.G0MA.F10 (dHCI)<br/>22RR.G0MB.F04 (dHCI GEO)<br/>22RR.G0MB.F05 (dHCI GEO) |
| East Burwood | 643 Highbury Road <br />East Burwood <br />VIC 3151 | O3EB.G0MG.F06 (dHCI) <br />O3EB.G0MG.F02 (Agg1) <br />O3EB.G0MA.F02 (Agg2) |
|   Rosebery   |      65 Epsom Road<br />Rosebery<br />NSW 2018      |               O2RO.G0BB.F10<br />O2RO.G0BB.F09               |

---

# Hyperconverged Infrastructure Beginners Guide

## SimpliVity Architecture

This section of the design document describes the implementation of the HPE SimpliVity 380 Hyperconverged Infrastructure

A SimpliVity Federation is created by connecting one or more OmniStack hosts to a vCenter Server cluster. Each host provides compute, storage and network resources to the Federation. A Federation uses the following mandatory networks:

*  Storage Network – Data Storage Network

* Federation Network – For Virtual Controller communication within a SimpliVity Federation

* Management Network – For Management access to the federation

Additional networks can (optionally) be created to connect to other virtual machine networks for virtual machine network communications. 

After configuring and adding additional nodes to the Federation, local storage on the nodes is consolidated into a single pool and is accessible from all OmniStack nodes in the cluster. Optionally, compute nodes (these are ESXi servers without any local storage) can be added to a cluster to increase the compute capacity of the cluster. vSphere datastores are created on the storage from vCenter and can be utilised by all nodes in the cluster.

To achieve VM high availability, all VM data is written simultaneously to two OmniStack hosts within the local cluster. If an OmniStack host fails, affected virtual machines can be restarted on another node. The vSphere HA cluster feature can also be used to automatically restart the VM. 

Intelligent virtual machine placement optimises the location of highly available virtual machine data within a cluster to ensure workload performance. This placement occurs automatically when creating or cloning a virtual machine or when restoring from a backup.

Most of this environment will be replaced by the dCHI environment, but portions will we retain for a subset of applications.

### Production Setup

The solution uses HPE SimpliVity Hyperconverged Infrastructure (HCI), which combines the entire IT stack into a single hyperconverged node, providing the agility and economics of the cloud with enterprise-grade performance and data protection. Optus utilises the all-flash, scalable, pre-integrated building blocks to host newly built OSS virtualised application workloads across the two data centres. 

![Optus OSS Infrastructure Diagrams v0.4](/assets/img/optus-handbook/Optus%20OSS%20Infrastructure%20Diagrams%20v0.4.png)

### NPTE

The Non-Production Test Environment (NPTE) is another component of the solution, which has been deployed in the Rosebery data centre. The NPTE environment has a mixed role, it is used to host Non-Production Optus vm workloads, as well as being used by HPE TSMC to test the installation of software updates and patches on like for like infrastructure. The NPTE environment will mimic the infrastructure and configuration of the Production Blacktown and Mascot solutions.

![Optus NPTE Diagrams v0.5](/assets/img/optus-handbook/Optus%20NPTE%20Diagrams%20v0.5.png)

---

## dHCI Architecture

HPE Nimble Storage dHCI is an intelligent platform with the flexibility of converged and the simplicity of HCI. It disaggregates compute and storage and integrates hyperconverged control to give enterprises simple infrastructure management on a flexible architecture.

The new HPE Nimble Storage dHCI solution will integrate with the existing Vmware environment hosting OSS workloads. Management and compute workloads will remain separated into dedicated dHCI clusters. Existing VM workloads, compute and management, will be migrated to the new dHCI platform, and the same HPE ITOC management stack of management and monitoring tools will be used.

## DHCI Network Block Diagram

![image-20230913210159838](/assets/img/optus-handbook/image-20230913210159838.png)

## dHCI Logical Networking

![image-20230913205545224](/assets/img/optus-handbook/image-20230913205545224.png)



---

#  Production Network

![Optus Physical Networking Prod](/assets/img/optus-handbook/Optus%20Physical%20Networking%20Prod.png)

## Blacktown Production & DMZ

![image-20230911151442356](/assets/img/optus-handbook/image-20230911151442356.png)

### Switches

| IP Address                      | Name                                                   | Description                      | Model  | Rack / RU                                  |
| ------------------------------- | ------------------------------------------------------ | -------------------------------- | ------ | ------------------------------------------ |
| 172.28.21.29                    | 22btpnwp0001<br />22btpnwp0002<br />IRF - 22btvirf0001 | FlexFabric 5945 - AGG switch IRF | JQ074A | G0ZJ.F04 RU38<br />G0ZK.F10 RU40           |
| 172.28.21.30                    | 22btpnwp0101<br />22btpnwp0102<br />IRF - 22btvirf0101 | FlexFabric 5940 - TOR switch IRF | JH390A | 22BT.G0ZK.F12 RU41<br />22BT.G0ZK.F12 RU40 |
| 172.28.21.24                    | 22btpnwp0201<br />22btpnwp0202<br />IRF - 22btvirf0201 | FlexFabric 5940 - TOR switch IRF | JH390A | 22BT.G0ZK.F10 RU41<br />22BT.G0ZK.F10 RU40 |
| 172.28.21.26                    | 22btpnwp0301<br />22btpnwp0302<br />IRF - 22btvirf0301 | FlexFabric 5945 - TOR switch IRF | JQ074A | 22BT.G0ZJ.F04 RU41<br />22BT.G0ZJ.F04 RU40 |
| 172.28.21.27<br />172.28.21.141 | 22btpnwo0101                                           | FlexFabric 5700 - TOR OOB switch | JG894A | 22BT.G0ZK.F12 RU39                         |

#### 

## Mascot Production

![rr - Agg switches as built Sept 2023[46 copy](/assets/img/optus-handbook/rr%20-%20Agg%20switches%20as%20built%20Sept%202023%5B46%5D%20copy.png)

---

### Switches

| IP Address                          | Name                                                   | Description                       | Model  | Rack/RU                                    |
| ----------------------------------- | ------------------------------------------------------ | --------------------------------- | ------ | ------------------------------------------ |
| 172.28.22.29                        | 22rrpnwp0001<br />22rrpnwp0002<br />IRF - 22btvirf0001 | FlexFabric 5945 - AGG switch IRF  | JQ074A | G0MB.F04 RU37<br />G0ME.F01 RU37           |
| 172.28.22.30                        | 22rrpnwp0101<br />22rrpnwp0102<br />IRF - 22btvirf0101 | FlexFabric 5940 - TOR switch IRF  | JH390A | 22RR.G0EA.F03 RU38<br />22RR.G0EA.F03 RU38 |
| 172.28.21.24                        | 22rrpnwp0201<br />22rrpnwp0202<br />IRF - 22btvirf0201 | FlexFabric 5940 - TOR switch IRF  | JH390A | 22RR.G0ME.F01 RU40<br />22RR.G0ME.F01 RU39 |
| 172.28.21.26                        | 22rrpnwp0301<br />22rrpnwp0302<br />IRF - 22btvirf0301 | FlexFabric 5945  - TOR switch IRF | JQ074A | 22RR.G0MB.F10 RU41<br />22RR.G0MB.F10 RU40 |
| <span style="color: red">TBC</span> | 22rrpnwp0401<br />22rrpnwp0402<br />IRF - 22btvirf0401 | FlexFabric 5945  - TOR switch IRF | JQ074A | 22RR.G0MB.F04 RU41<br />22RR.G0MB.F04 RU40 |
| <span style="color: red">TBC</span> | 22rrpnwo0101                                           | TOR OOB switch                    | JG894A | 22RR.G0EA.F03 RU36                         |
| 172.28.21.25                        | 22rrpnwo0301                                           | TOR OOB switch                    | JH390A | 22RR.G0MB.F10 RU39                         |
| 22rrvirf0401                        | 22rrpnwo0401                                           | TOR OOB switch                    | JG510A | 22RR.G0MB.F04 RU39                         |

## Mascot GEO

![Optus GEO-MB](/assets/img/optus-handbook/Optus%20GEO-MB.png)

### Routing 

The routing for Geo is done directly onto the AGG Switches via BGP peers terminated onto the VRF. From here vlans are used to trunk the traffic to the GEO Cluster

### Switches

| IP Address    | Name                                                   | Description                       | Model  | Rack/RU                                    |
| ------------- | ------------------------------------------------------ | --------------------------------- | ------ | ------------------------------------------ |
| 172.19.175.24 | 22rrpnwp0401<br />22rrpnwp0402<br />IRF - 22btvirf0401 | FlexFabric 5945  - TOR switch IRF | JQ074A | 22RR.G0MB.F04 RU41<br />22RR.G0MB.F04 RU40 |
|               | 22rrpnwo0401                                           | TOR OOB switch                    | JG510A | 22RR.G0MB.F04 RU39                         |

---

## East Burwood Production and DMZ

![EB - Agg switches as built Sept 2023[46](/assets/img/optus-handbook/EB%20-%20Agg%20switches%20as%20built%20Sept%202023%5B46%5D.png)



### Switches

| IP Address    | Name                                                   | Description                      | Model  | Rack / RU                              |
| ------------- | ------------------------------------------------------ | -------------------------------- | ------ | -------------------------------------- |
| 172.19.175.75 | o3ebpnwp0001<br />o3ebpnwp0002<br />IRF - o3ebvirf0001 | FlexFabric 5945 - AGG switch IRF | Q074AJ | G0MG.F02 RU24<br />G0MA.F02 RU24       |
| 172.19.175.71 | o3ebpnwp0101<br />o3ebpnwp0102<br />IRF - o3ebvirf0101 | FlexFabric 5940-TOR switch IRF   | JH390A | O3EB.G0MG.F06.41<br />O3EB.G0MG.F06.40 |
| 172.28.23.100 | o3ebpnwo0101[^fn2]                                     | FlexFabric 5700 - TOR OOB switch | JG894A | O3EB.G0MG.F06.39                       |

---

## Storage Networking

### Nimble Storage

The existing nimble storage arrays will be maintained and keep there current role

Virtual machine access to the Nimble HF40 storage is via the iSCSI protocol, where virtual machines access the block storage using iSCSI initiator software across a dedicated storage network. Storage replication is configured between two production array’s to allow Optus OSS the capability of replicating critical workloads between the two production sites, in either direction.

#### Blacktown

| VLAN | IP Address        | VRF                                 | Function                    |
| ---- | ----------------- | ----------------------------------- | --------------------------- |
| 20   | 172.24.201.224/28 | <span style="color: red">TBC</span> | Storage replication network |
| 21   | 198.18.1.0/26     | Local Only                          | iSCSI_A                     |
| 22   | 192.18.1.64/26    | Local Only                          | iSCSI_B                     |
| 36   | 172.28.21.192/27  | M333:T2:DC2-INFRA2                  | Management network          |

##### Mascot

| VLAN | IP Address        | VRF                                 | Function                    |
| ---- | ----------------- | ----------------------------------- | --------------------------- |
| 20   | 172.24.201.240/28 | <span style="color: red">TBC</span> | Storage replication network |
| 21   | 198.18.1.0/26     | Local Only                          | iSCSI_A                     |
| 22   | 192.18.1.64/26    | Local Only                          | iSCSI_BiSCSI_B              |
| 36   | 172.28.22.192/27  | M333:T2:DC2-INFRA2                  | Management network          |

### dHCI ISCSI Networking

The HPE Alletra storage arrays will be configured with management, replication, and iSCSI networks. iSCSI networking will consist of two redundant networks, iSCSI_A and iSCSI_B, where cabling and networking configuration will ensure that redundant networking pathing is available for block storage presented to iSCSI clients (physical and virtual).

Optus has requested that iSCSI connections to the Alletra arrays be made available to the clients cross-site. That is, VMs located in Mascot can access Alletra iSCSI block storage in Blacktown. And VMs in Blacktown can access Allera iSCSI block storage in Mascot. Optus will provide routable networks for these cross-site iSCSI connections. Local iSCSI connections for VM clients and dHCI ESXi hosts will be non-routable networks maintained within the HPE network infrastructure.[^fn5]

All network interfaces connected to the storage interface must be set to have their mtu to 9000.

#### Blacktown

| VLAN | IP Address        | VRF                                 | Function              |
| ---- | ----------------- | ----------------------------------- | --------------------- |
| 23   | 198.18.4.0/25     | Local Only                          | iSCSI_A - Prod        |
| 24   | 198.18.5.0/24     | Local Only                          | iSCSI_B - Prod        |
| 27   | 198.18.4.0/25     | Local Only                          | iSCSI_A - DMZ         |
| 28   | 198.18.5.0/25     | Local Only                          | iSCSI_B - DMZ         |
| 25   | 10.103.3.0/26     | <span style="color: red">TBC</span> | ISCSI_A - Prod Routed |
| 26   | 10.103.3.64/26    | <span style="color: red">TBC</span> | ISCSI_B - Prod Routed |
| 36   | 172.28.21.192/26  | M333:T2:DC2-INFRA2                  | Management            |
| 40   | 172.24.201.224/28 | M368:T2:DC2-REPLICATION             | Replication           |

---

#### Mascot 

| VLAN | IP Address        | VRF                                 | Function              |
| ---- | ----------------- | ----------------------------------- | --------------------- |
| 23   | 198.18.4.0/25     | Local Only                          | iSCSI_A- Prod         |
| 24   | 198.18.5.0/24     | Local Only                          | iSCSI_B - Peod        |
| 51   | 198.18.7.0/24     | Local Only                          | SCSI_A - GEO          |
| 52   | 198.18.8.0/24     | Local Only                          | SCSI_A - GEO          |
| 25   | 10.103.3.128/26   | <span style="color: red">TBC</span> | ISCSI_A - Prod Routed |
| 26   | 10.103.3.192/26   | <span style="color: red">TBC</span> | ISCSI_B - Prod Routed |
| 36   | 172.28.22.192/26  | M333:T2:DC2-INFRA2                  | Management - Prod     |
| 37   | 172.19.113.64/26  | M333:T2:DC2-INFRA2                  | Management - Geo      |
| 40   | 172.24.201.240/28 | M368:T2:DC2-REPLICATION             | Replication           |

#### Mascot -  Geo

| VLAN ID | Description        | IP CIDR         |
| ------- | ------------------ | --------------- |
| 48      | GEO VM NFS Network | 10.107.124.0/24 |
| 49      | GEO VM App network | 172.19.111.0/24 |
| 50      | GEO VM OAM network | 172.19.112.0/24 |
| 51      | GEO Alletra iSCSIA | 198.18.7.0/24   |
| 52      | GEO Alletra iSCSIB | 198.18.8.0/24   |

####  East Burwood

| VLAN                                | IP Address                          | VRF                     | Function      |
| ----------------------------------- | ----------------------------------- | ----------------------- | ------------- |
| 23                                  | 198.18.4.0/25                       | Local Only              | iSCSI_A       |
| 24                                  | 198.18.5.0/25                       | Local Only              | iSCSI_B       |
| 36                                  | 172.28.23.192/26                    | M333:T2:DC2-INFRA2      | Management    |
| 40                                  | 172.19.175.48/28                    | M368:T2:DC2-REPLICATION | Replication   |
| <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | Local Only              | iSCSI_A - DMZ |
| <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | Local Only              | iSCSI_B - DMZ |

---

## Data Networking

The following VLANs have been created on their respective sites. Please attempt to keep the VLANs uniform across sites as it makes troubleshooting / migrating VMs easier

### Blacktown

| VLAN      | IP Address                    | VRF                                 | VLAN Description                                     |
| --------- | ----------------------------- | ----------------------------------- | ---------------------------------------------------- |
| 20        | 172.24.102.224/28             | M333:T2:DC2-INFRA2                  | External Storage Replication                         |
| 21        | 198.18.1.0/26                 | (not routed)                        | Nimble External Storage<br/>iSCSI_A (local)          |
| 22        | 198.18.1.64/26                | (not routed)                        | Nimble External Storage<br/>iSCSI_B (local)          |
| 23        | 198.18.4.0/24                 | (not routed)                        | Alletra External Storage<br/>iSCSI_A (local)         |
| 24        | 198.18.5.0/24                 | (not routed)                        | Alletra External Storage<br/>iSCSI_B (local)         |
| 25        | 10.103.3.0/26                 | <span style="color: red">TBC</span> | Alletra External Storage <br />iSCSI_A (cross-site)  |
| 26        | 10.107.3.64/26                | <span style="color: red">TBC</span> | Alletra External Storage <br />iSCSI_B  (cross-site) |
| 27        | 198.18.5.0/26                 | <center>-</center>                  | ExternalStorage<br/>iSCSI_A_DMZ (local)              |
| 28        | 198.18.5.64/26                | <center>-</center>                  | ExternalStorage<br/>iSCSI_A_DMZ (local)              |
| 29        |                               | <center>-</center>                  | NSX RTEP                                             |
| 30        | 172.19.175.32/29              | M466:OSSCLOUD:GEOCONNECT            | NSX VTEP                                             |
| 31        |                               | <center>-</center>                  | SVT Federation (Mgmt)                                |
| 32        |                               |                                     | SVT Storage (Mgmt)                                   |
| 33        | 172.28.21.65/26               | M331:T2:DC2-INFRA1                  | Management OOB                                       |
| 34        |                               | <center>-</center>                  | SVT Federation (Compute)                             |
| 35        |                               | <center>-</center>                  | SVT Storage (Compute)                                |
| 36        | 172.28.21.193/26              | M333:T2:DC2-INFRA2                  | In-Band Compute                                      |
| 40        | 172.24.201.225/28             | M368:T2:DC2-REPLICATION             | Alletra Replication                                  |
| 41        |                               | <center>-</center>                  | SVT Federation (DMZ)                                 |
| 42        |                               | <center>-</center>                  | SVT Storage (DMZ)                                    |
| 46        | 172.28.23.65/28               | M337:T1:DC2-INFRA1                  | In-Band Compute (DMZ)                                |
| 150       | 172.28.21.16/28               | M331:T2:DC2-INFRA1                  | Management OOB                                       |
| 151       | 172.28.21.128/26              | M333:T2:DC2-INFRA2                  | Compute OOB                                          |
| 152       | 172.28.21.128/26              | M337:T1:DC-INFRA1                   | DMZ OOB                                              |
| 153       | 172.19.175.0/28               | M331:T2:DC2-INFRA1                  | Management OOB                                       |
| 1000-1099 | RESERVED FOR OPTUS ALLOCATION | <center>-</center>                  | BGP PEERING WITH OPTUS                               |
| 1100-1199 | RESERVED FOR OPTUS ALLOCATION | <center>-</center>                  | BGP PEERING WITH OPTUS                               |

---

### Mascot

#### OSS Cloud - Production

| VLAN      | IP Address                    | VRF                                 | VLAN Description                                     |
| --------- | ----------------------------- | ----------------------------------- | ---------------------------------------------------- |
| 20        | 172.24.102.224/28             | M333:T2:DC2-INFRA2                  | External Storage Replication                         |
| 21        | 198.18.1.0/26                 | (not routed)                        | Nimble External Storage<br/>iSCSI_A (local)          |
| 22        | 198.18.1.64/26                | (not routed)                        | Nimble External Storage<br/>iSCSI_B (local)          |
| 23        | 198.18.4.0/24                 | (not routed)                        | Alletra External Storage<br/>iSCSI_A (local)         |
| 24        | 198.18.5.0/24                 | (not routed)                        | Alletra External Storage<br/>iSCSI_B (local)         |
| 25        | 10.103.3.0/26                 | <span style="color: red">TBC</span> | Alletra External Storage <br />iSCSI_A (cross-site)  |
| 26        | 10.107.3.64/26                | <span style="color: red">TBC</span> | Alletra External Storage <br />iSCSI_B  (cross-site) |
| 27        | 198.18.5.0/26                 | (not routed)                        | ExternalStorage<br/>iSCSI_A_DMZ (local)              |
| 28        | 198.18.5.64/26                | (not routed)                        | ExternalStorage<br/>iSCSI_A_DMZ (local)              |
| 29        |                               | M466:OSSCLOUD:GEOCONNECT            | NSX VTEP                                             |
| 30        |                               | (not routed)                        | NSX RTEP                                             |
| 31        |                               | (not routed)                        | Federation<br/>(NPTE Management Cluster)             |
| 32        |                               |                                     |                                                      |
| 33        | 172.28.21.65/26               | M331:T2:DC2-INFRA1                  | Management OOB                                       |
| 34        | 198.18.3.0/26                 | (not routed)                        | Federation (Compute)                                 |
| 35        | 198.18.3.64/26                | (not routed)                        | Storage (Compute)                                    |
| 36        | 172.28.21.193/26              | M333:T2:DC2-INFRA2                  | In-Band Compute                                      |
| 40        | 172.24.201.241/28             | M368:T2:DC2-REPLICATION             | Alletra Replication                                  |
| 48        | 10.107.124.1/24               | M333:T2:DC2-INFRA2 (temp VLAN)      | <span style="color: red">TBC</span>                  |
| 150       | 172.28.22.16/28               | M331:T2:DC2-INFRA1                  | OUT  Of Band Management                              |
| 151       | 172.28.21.128/26              | M333:T2:DC2-INFRA2                  | Out of Band Mgmt (Compute)                           |
| 1000-1099 | RESERVED FOR OPTUS ALLOCATION | NSX-T BGP PEERING WITH OPTUS        | BGP PEERING WITH OPTUS                               |
| 1100-1199 | RESERVED FOR OPTUS ALLOCATION | NSX-T BGP PEERING WITH OPTUS        | BGP PEERING WITH OPTUS                               |

---

#### GEO - Production

| VLAN      | IP Address                    | VRF                          | VLAN Description                             |
| --------- | ----------------------------- | ---------------------------- | -------------------------------------------- |
| 20        | 172.24.102.224/28             | M333:T2:DC2-INFRA2           | External Storage Replication                 |
| 21        | 198.18.1.0/26                 | (not routed)                 | Nimble External Storage<br/>iSCSI_A (local)  |
| 22        | 198.18.1.64/26                | (not routed)                 | Nimble External Storage<br/>iSCSI_B (local)  |
| 23        | 198.18.4.0/24                 | (not routed)                 | Alletra External Storage<br/>iSCSI_A (local) |
| 24        | 198.18.5.0/24                 | (not routed)                 | Alletra External Storage<br/>iSCSI_B (local) |
| 27        | 198.18.5.0/26                 | (not routed)                 | ExternalStorage<br/>iSCSI_A_DMZ (local)      |
| 28        | 198.18.5.64/26                | (not routed)                 | ExternalStorage<br/>iSCSI_A_DMZ (local)      |
| 31        |                               | (not routed)                 | Federation<br/>(NPTE Management Cluster)     |
| 37        | 172.19.113.64/26              | M333:T2:DC2-INFRA2           | In-Band Compute                              |
| 38        | 172.19.113.28/25              | M333:T2:DC2-INFRA2           | Qumulo - In-Band Compute                     |
| 52        | 198.18.8.0/24                 | ExternalStorageiSCSI_B_GEO   | Alletra External Storage<br />ISCSI B        |
| 51        | 198.18.7.0/24                 | ExternalStorage iSCSI_A_GEO  | Alletra External Storage<br />ISCSI A        |
| 1000-1099 | RESERVED FOR OPTUS ALLOCATION | NSX-T BGP PEERING WITH OPTUS | BGP PEERING WITH OPTUS                       |
| 1100-1199 | RESERVED FOR OPTUS ALLOCATION | NSX-T BGP PEERING WITH OPTUS | BGP PEERING WITH OPTUS                       |
| 1601      | 172.19.113.0/26               | M333:T2:DC2-INFRA2           | Compute OOB                                  |

---

### East Burwood

| VLAN      | IP Address                    | VRF                           | VLAN Description                             |
| --------- | ----------------------------- | ----------------------------- | -------------------------------------------- |
| 20        | 172.24.102.224/28             | M333:T2:DC2-INFRA2            | External Storage Replication                 |
| 21        | 198.18.1.0/26                 | (not routed)                  | Nimble External Storage<br/>iSCSI_A (local)  |
| 22        | 198.18.1.64/26                | (not routed)                  | Nimble External Storage<br/>iSCSI_B (local)  |
| 23        | 198.18.4.0/24                 | (not routed)                  | Alletra External Storage<br/>iSCSI_A (local) |
| 24        | 198.18.5.0/24                 | (not routed)                  | Alletra External Storage<br/>iSCSI_B (local) |
| 27        | 198.18.5.0/26                 | (not routed)                  | ExternalStorage<br/>iSCSI_A_DMZ (local)      |
| 28        | 198.18.5.64/26                | (not routed)                  | ExternalStorage<br/>iSCSI_A_DMZ (local)      |
| 29        |                               | M466:OSSCLOUD:GEOCONNECT      | NSX VTEP                                     |
| 30        |                               | (not routed)                  | NSX RTEP                                     |
| 33        | 172.19.175.97/27              | M331:T2:DC2-INFRA1            | InBand Management                            |
| 36        | 172.28.23.193/26              | M333:T2:DC2-INFRA2            | InBand Management (Compute)                  |
| 46        | 172.28.23.161/27              | M337:T1:DC2-INFRA1            | InBand Management (DMZ)                      |
| 1000-1099 | RESERVED FOR OPTUS ALLOCATION | RESERVED FOR OPTUS ALLOCATION | PtP Peering                                  |
| 1100-1199 | RESERVED FOR OPTUS ALLOCATION | RESERVED FOR OPTUS ALLOCATION | PtP Peering                                  |
| 1571      | 172.19.175.65/27              | M331:T2:DC2-INFRA1            | OoB Support (Mgmt)                           |
| 1572      | 172.28.23.97/27               | M333:T2:DC2-INFRA2            | OoB Support (Compute)                        |
| 1573      | 172.19.175.129/28             | M337:T1:DC2-INFRA1            | OoB Support (DMZ)                            |
|           |                               |                               |                                              |
|           |                               |                               |                                              |

---

## Routing

### BGP

BGP peering is established to provide the higher layer routing function of EVPN. EVPN’s function is to enable logical, layer-2 segments that can span the data centres. BGP peering will occur over the Optus WAN. Private BGP AS numbers are used and are unique to each site so that eBGP can be used.

| Site           | BGP AS |
| -------------- | ------ |
| Optus Upstream | 4804   |
| Mascot         | 64849  |
| East Burwood   | 65477  |
| Blacktown      | 64848  |
|                |        |

### VRFs

| VRF                      | Sites      | Description           |
| ------------------------ | ---------- | --------------------- |
| M333:T2:DC2-INFRA2       | Blacktown, |                       |
| M337:T1:DC2-INFRA1       | Blacktown, |                       |
| M368:T2:DC2-REPLICATION  | Blacktown, |                       |
| M466:OSSCLOUD:GEOCONNECT | Blacktown, | VRF FOR GEOCONNECT () |



### Blacktown

#### 22rrvirf0001

| Peer Node | IP Address     | VRF                      | VLAN |
| --------- | -------------- | ------------------------ | ---- |
| bla5.cr   | 172.19.175.173 | M331:T2:DC2-INFRA1       | 11   |
| bla6.cr   | 172.19.175.177 | M331:T2:DC2-INFRA1       | 16   |
| bla5.cr   | 172.19.175.181 | M333:T2:DC2-INFRA2       | 12   |
| bla6.cr   | 172.19.175.185 | M333:T2:DC2-INFRA2       | 17   |
| bla5.cr   | 172.19.175.189 | M368:T2:DC2-REPLICATION  | 13   |
| bla6.cr   | 172.19.175.193 | M368:T2:DC2-REPLICATION  | 18   |
| bla5.cr   | 172.19.175.197 | M337:T1:DC2-INFRA1       | 14   |
| bla6.cr   | 172.19.175.201 | M337:T1:DC2-INFRA1       | 19   |
| bla5.cr   | 172.28.23.205  | M466:OSSCLOUD:GEOCONNECT | 15   |
| bla5.cr   | 172.28.23.205  | M466:OSSCLOUD:GEOCONNECT | 20   |

---

#### 22rrvirf0101

| Peer Node | IP Address    | VRF                      | VLAN |
| --------- | ------------- | ------------------------ | :--- |
| bla5.cr   | 172.28.23.2   | M331:T2:DC2-INFRA1       | 11   |
| bla6.cr   | 172.28.23.10  | M331:T2:DC2-INFRA1       | 16   |
| bla5.cr   | 172.28.23.10  | M333:T2:DC2-INFRA2       | 12   |
| bla6.cr   | 172.28.23.14  | M333:T2:DC2-INFRA2       | 17   |
| bla5.cr   | 172.28.23.18  | M368:T2:DC2-REPLICATION  | 13   |
| bla6.cr   | 172.28.23.22  | M368:T2:DC2-REPLICATION  | 18   |
| bla5.cr   | 172.28.23.138 | M466:OSSCLOUD:GEOCONNECT | 15   |
| bla5.cr   | 172.28.23.142 | M466:OSSCLOUD:GEOCONNECT | 20   |

### Mascot

| Peer Node | IP Address     | VRF                      | VLAN |
| --------- | -------------- | ------------------------ | ---- |
| mas5.cr   | 172.19.175.173 | M331:T2:DC2-INFRA1       | 11   |
| mas6.cr   | 172.19.175.177 | M331:T2:DC2-INFRA1       | 16   |
| mas5.cr   | 172.19.175.181 | M333:T2:DC2-INFRA2       | 12   |
| mas6.cr   | 172.19.175.185 | M333:T2:DC2-INFRA2       | 17   |
| mas5.cr   | 172.19.175.189 | M368:T2:DC2-REPLICATION  | 13   |
| mas6.cr   | 172.19.175.193 | M368:T2:DC2-REPLICATION  | 18   |
| mas5.cr   | 172.28.23.205  | M466:OSSCLOUD:GEOCONNECT | 15   |
| mas5.cr   | 172.28.23.205  | M466:OSSCLOUD:GEOCONNECT | 20   |

### East Burwood

| Peer Node | IP Address     | VRF                      | VLAN |
| --------- | -------------- | ------------------------ | ---- |
| meb5.cr   | 172.19.175.173 | M331:T2:DC2-INFRA1       | 11   |
| meb6.cr   | 172.19.175.177 | M331:T2:DC2-INFRA1       | 16   |
| meb5.cr   | 172.19.175.181 | M333:T2:DC2-INFRA2       | 12   |
| meb6.cr   | 172.19.175.185 | M333:T2:DC2-INFRA2       | 12   |
| meb5.cr   | 172.19.175.189 | M368:T2:DC2-REPLICATION  | 13   |
| meb6.cr   | 172.19.175.193 | M368:T2:DC2-REPLICATION  | 18   |
| meb5.cr   | 172.28.23.205  | M466:OSSCLOUD:GEOCONNECT | 15   |
| meb6.cr   | 172.28.23.209  | M466:OSSCLOUD:GEOCONNECT | 12   |

---

# NPTE

Whilst this environment has the name Non-Production Test Bed, some applications running here are connected to and used with production systems. As such, operations in this environment will be performed under change management.

## Rosebury

### Switches

| IP Address    | Name                                                   | Description                      | Model  | Rack/RU                                    |
| ------------- | ------------------------------------------------------ | -------------------------------- | ------ | ------------------------------------------ |
| 10.201.153.42 | o2ropnwp0001<br />o2ropnwp0002<br />IRF - 02rovirf0001 | FlexFabric 5940 IRF TOR          | JH390A | O2RO.G0BB.F09 RU41<br />O2RO.G0BB.F09 RU40 |
| 10.201.153.43 | o2ropnwo0101                                           | FlexFabric 5700 - TOR OOB switch | JG894A | O2RO.G0BB.F09 RU41                         |

## Storage Networking

| VLAN | IP Address       | VRF                | Function   |
| ---- | ---------------- | ------------------ | ---------- |
| 23   | 198.18.4.0/25    | Local Only         | iSCSI_A    |
| 24   | 198.18.5.0/25    | Local Only         | iSCSI_B    |
| 36   | 10.201.161.64/27 | M333:T2:DC2-INFRA2 | Management |

## Data Networking

| VLAN                  | IP Address                    | VRF                           | VLAN Description                             |
| --------------------- | ----------------------------- | ----------------------------- | -------------------------------------------- |
| 20                    | 172.24.102.224/28             | M333:T2:DC2-INFRA2            | External Storage Replication                 |
| 21                    | 198.18.1.0/26                 | (not routed)                  | Nimble External Storage<br/>iSCSI_A (local)  |
| 22                    | 198.18.1.64/26                | (not routed)                  | Nimble External Storage<br/>iSCSI_B (local)  |
| 23                    | 198.18.4.0/24                 | (not routed)                  | Alletra External Storage<br/>iSCSI_A (local) |
| 24                    | 198.18.5.0/24                 | (not routed)                  | Alletra External Storage<br/>iSCSI_B (local) |
| 31                    | 192.168.216.96/28             |                               | Federation (Mgmt.)                           |
| 32                    | 192.168.216.0/27              |                               | Storage (Mgmt.)                              |
| 33                    | 10.201.161.0/26               |                               | In-Band Mgmt. (Inc. VTEPS & Controllers)     |
| 35                    | 192.168.216.64/27             |                               | Storage (Compute)                            |
| 36                    | 10.201.161.64/27              |                               | In-Band Mgmt (Compute)                       |
| 34                    | 192.168.216.112/28            |                               | Federation (Compute)                         |
| 150                   | 10.201.153.32/28              |                               | Out-of-Band Mgmt.                            |
| 151                   | 10.201.153.48/28              |                               | Out of Band Mgmt (Compute)                   |
| 1000<br />-<br />1099 | RESERVED FOR OPTUS ALLOCATION | RESERVED FOR OPTUS ALLOCATION | PtP Peering                                  |



## Routing

### BGP

BGP peering is established to provide the higher layer routing function of EVPN. EVPN’s function is to enable logical, layer-2 segments that can span the data centres. BGP peering will occur over the Optus WAN. Private BGP AS numbers are used and are unique to each site so that eBGP can be used.

| Site           | BGP AS     |
| -------------- | ---------- |
| Optus Upstream | 4804       |
| Rosebury       | 4259774479 |



| Peer Node | IP Address        | VRF                                             | VLAN |
| --------- | ----------------- | ----------------------------------------------- | ---- |
| PE1       | 10.201.161.112/30 | Management/Default<br />M348:T2:DC2-NPTE-INFRA1 | 11   |
| PE2       | 10.201.161.120/30 | Management/Default<br />M348:T2:DC2-NPTE-INFRA1 | 13   |
| PE1       | 10.201.161.116/30 | M349:T2:DC2-NPTE-INFRA2                         | 12   |
| PE2       | 10.201.161.124/30 | M349:T2:DC2-NPTE-INFRA2                         | 14   |

| **Host Name - A End**                       | **Rack locations - A **End | **Host Name - B End**                                | **Rack locations - B End** | **Quantity**           |
| ------------------------------------------- | -------------------------- | ---------------------------------------------------- | -------------------------- | ---------------------- |
| O2ROPNWP0102<br/>HPE FlexFabric 5940 Switch | O2RO.G0ZK.F12.R40          | Optus WAN D-MAS30<br/>Te0/3/0/28                     | O2RO.G097.F13.003          | 1 x SM LC<br/>10Gb     |
| O2ROPNWP0101<br/>HPE FlexFabric 5940 Switch | O2RO.G0ZK.F12.R39          | O2ROPNWP0102-Eth3 Optus WAN D-MAS31<br/>Te0/3/0/28   | O2RO.G097.F14.003          | 1 x SM LC<br/>10Gb     |
| O2ROPNWO0101<br/>HPE FlexFabric 5700 Switch | O2RO.G0ZK.F12.R38          | Rosebery Switch-1<br/>ROS1CR11-101.nx<br/>Eth101/1/5 | O2RO.G097.F13.028          | 1 x Cat5e<br/>RJ45 1Gb |



---

# NSX-T

## NSX-T Technical Stuff

### TLDR - How it Works

![Optus Networking - NSX](/assets/img/optus-handbook/Optus%20Networking%20-%20NSX.png)





The NSX Manager resides on the NPTE management cluster VLAN. Each ESX Compute host uses multiple VTEP interfaces and each Edge node requires a single management IP. These are referred to as ‘Host Overlay’ networks.

For the NPTE guest workloads, two VLAN’s are required for the logical uplinks to the Optus PE Lab Router for each application VRF. These are referred to as ‘Edge Overlay’ VLAN’s.

Host Overlay is a transport VLAN used for host-to-host communication between guest workloads. Very briefly, when a guest communicates with another guest on another host, the data is encapsulated inside a Geneve protocol packet and then sent to the other host, where it is decapsulated and forwarded to the guest. Similarly, the Edge Overlay VLAN is used to transport data between Guests and Edge appliances such as a Tier-0 virtual router.

*Note: When transmitting packets between Guests and Edge virtual machines in an NSX-T environment, this is all transmitted as Geneve encapsulated packets. The underlying architecture dictates all Guest traffic resides in the ‘Overlay” and is transmitted via the ‘Underlay’ between hosts. In this way, the underlay has no visibility of the Guest environment and hence is virtualised.*

### Edge Nodes

![NSX - Edge Nodes](/assets/img/optus-handbook/NSX%20-%20Edge%20Nodes.png)

### The Geneve Protocol

The GENEVE header looks a lot like VXLAN and has the following structure:

* A compact tunnel header is encapsulated in UDP over IP.

* A small fixed tunnel header is used to provide control information, as well as a base level of functionality and interoperability.
* Variable length options are available for making possible to implement future innovations.

![Geneve Protocol](/assets/img/optus-handbook/Geneve%20Protocol.png)

The size of the GENEVE header is variable.

NSX-T uses GENEVE (**GE**neric **N**Etwork **V**irtualization **E**ncapsulation) as a tunneling protocol that preserves traditional offload capabilities available on NICs (Network Interface Controllers) for the best performance. Additional metadata can be added to overlay headers and allows to improve context differencing for processing information such as end-to-end telemetry, data tracking, encryption, security etc. on the data transferring layer. Additional information in the metadata is called TLV (Type, Length, Value). GENEVE is developed by VMware, Intel, Red Hat and Microsoft. GENEVE is based on the best concepts of VXLAN, STT and NVGRE encapsulation protocols.

The MTU value for Jumbo frames must be at least 1700 bytes when using GENEVE encapsulation that is caused by the additional metadata field of variable length for GENEVE headers 

### Transport Zones

Transport zones define the limits of logical networks distribution. Each transport zone is linked to its NSX Switch (N-VDS). Transport zones for NSX-T are not linked to clusters. Due to GENEVE encapsulation, there are two types of transport zones for VMware NSX-T: Overlay or VLAN.

### Routing in NSX

![NSX Routing](/assets/img/optus-handbook/NSX%20Routing.png)

NSX-T uses a two-tier distributed routing model for resolving issues explained above. Both Tier0 and Tier1 are created on the Transport nodes, the latter of which is not necessary, but is intended for improving scalability.

Traffic is transmitted by using the most optimal path, as routing is then performed on the ESXi or KVM hypervisor on which the VMs are running. The only case when a fixed point of routing must be used is when connecting to external networks. There are separate Edge nodes deployed on servers running hypervisors.

Additional services such as BGP, NAT, and Edge Firewall can be enabled on Edge nodes, which can in turn be combined into a cluster for improving availability. What’s more, NSX-T also provides faster failure detection. In simple terms, the best means for distributing routing is routing inside the virtualized infrastructure.

### Firewall

NSX-T allows you to apply rules granularly, resulting in transport nodes being utilised more rationally. For example, you can apply rules based on the following objects: logical switch, logical port, NSGroup. This feature can be used to reduce rule-set configuration on the logical switch, logical port or NSGroup instances to achieve higher efficiency and optimisation levels. You can also save scale space and rule lookup cycles, in addition to hosting multi-tenancy deployment, and apply tenant-specific rules (rules applied to workloads of the appropriate tenant).

The process of creating and applying the rules is quite similar for both NSX-v and NSX-T. The difference is that the policies created for NSX-T are sent to all Controllers where rules are converted to IP addresses

### Edge Node Sizing 

| **Appliance Size**   | **Memory ** | **vCPU** | **Disk Space ** | **VM Hardware Version**           | **Notes**                                                    |
| -------------------- | ----------- | -------- | --------------- | --------------------------------- | ------------------------------------------------------------ |
| NSX Edge Small       | 4 GB        | 2        | 200 GB          | 11 or later (vSphere6.0 or later) | Proof-of-concept deployments only. <br /><br />**Note:**L7 rules for firewall, load balancing and so on are not realized on a Tier-1 gateway if you deploy a small sized NSX Edge VM. |
| NSX Edge Medium      | 8 GB        | 4        | 200 GB          | 11 or later (vSphere6.0 or later) | Suitable when only L2 through L4 features such as NAT, routing, L4 firewall, L4 load balancer are required and the total throughput requirement is less than 2 Gbps. |
| NSX Edge Large       | 32 GB       | 6        | 200 GB          | 11 or later (vSphere6.0 or later) | Suitable when only L2 through L4 features such as NAT, routing, L4 firewall, L4 load balancer are required and the total throughput is 2 ~ 10 Gbps. It is also suitable when L7 load balancer, for example, SSL offload is required.See [Scaling Load Balancer Resources ](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.1/administration/GUID-19B12230-8BF4-4AF7-9EB7-3701B0A0A439.html)in the NSX-T Data Center Administration Guide. For more information about what the different load balance sizes and NSX Edge form factors can support, see [https://configmax.vmware.com](https://configmax.vmware.com/). |
| NSX Edge Extra Large | 64 GB       | 8        | 200 GB          | 11 or later (vSphere6.0 or later) | Suitable when the total throughput required is multiple Gbps for L7 load balancer and VPN.See [Scaling Load Balancer Resources ](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.1/administration/GUID-19B12230-8BF4-4AF7-9EB7-3701B0A0A439.html)in the NSX-T Data Center Administration Guide. For more information about what the different load balance sizes and NSX Edge form factors can support, see [https://configmax.vmware.com](https://configmax.vmware.com/). |

---

## Production

![Optus Logical Networking](/assets/img/optus-handbook/Optus%20Logical%20Networking.png)

### Blacktown

| Name          | VLAN                                | IP Addresses                        | VRF                                 | Description                 | CLUSTER    |
| ------------- | ----------------------------------- | ----------------------------------- | ----------------------------------- | --------------------------- | ---------- |
| 22btvnsxgvip  | <span style="color: red">TBC</span> | 172.28.21.119                       | <span style="color: red">TBC</span> | Global Manager floating VIP | Management |
| 22btvnsxg01   | <span style="color: red">TBC</span> | 172.28.21.120                       | <span style="color: red">TBC</span> | Global Manager 1 Active     | Management |
| 22btvnsxg02   | <span style="color: red">TBC</span> | 172.28.21.121                       | <span style="color: red">TBC</span> | Global Manager 2 Active     | Management |
| 22btvnsxg03   | <span style="color: red">TBC</span> | 172.28.21.122                       | <span style="color: red">TBC</span> | Global Manager 3 Active     | Management |
| 22btvnsxmvip  | <span style="color: red">TBC</span> | 172.28.21.123                       | <span style="color: red">TBC</span> | Local Manager VIP           | Management |
| 22btvnsxm01   | <span style="color: red">TBC</span> | 172.28.21.124                       | <span style="color: red">TBC</span> | Local Manager 1             | Management |
| 22btvnsxm02   | <span style="color: red">TBC</span> | 172.28.21.125                       | <span style="color: red">TBC</span> | Local Manager 2             | Management |
| 22btvnsxm03   | <span style="color: red">TBC</span> | 172.28.21.126                       | <span style="color: red">TBC</span> | Local Manager 3             | Management |
| 22btvnsxedg01 | <span style="color: red">TBC</span> | 172.28.21.202                       | <span style="color: red">TBC</span> | Prod Edge Node 1            | Compute    |
| 22btvnsxedg02 | <span style="color: red">TBC</span> | 172.28.21.206                       | <span style="color: red">TBC</span> | Prod Edge Node 2            | Compute    |
| 22btvnsxedg10 | <span style="color: red">TBC</span> | 172.28.21.249                       | <span style="color: red">TBC</span> | DMZ Edge Node 1             | Compute    |
| 22btvnsxedg11 | <span style="color: red">TBC</span> | 172.28.21.250                       | <span style="color: red">TBC</span> | DMZ Edge Node 2             | Compute    |
| 22btvnsxedg2o | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | Stretched Edge Node 1       | Compute    |
| 22btvnsxedg21 | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | Stretched Edge Node 1       | Compute    |

### Mascot

| Name          | VLAN                                | IP Addresses                        | VRF                                 | Description                 | CLUSTER    |
| ------------- | ----------------------------------- | ----------------------------------- | ----------------------------------- | --------------------------- | ---------- |
| 22rrvnsxgvip  | <span style="color: red">TBC</span> | 172.28.22.119                       | <span style="color: red">TBC</span> | Global Manager floating VIP | Management |
| 22rrvnsxg01   | <span style="color: red">TBC</span> | 172.28.22.120                       | <span style="color: red">TBC</span> | Global Manager 1 Active     | Management |
| 22rrvnsxg02   | <span style="color: red">TBC</span> | 172.28.22.121                       | <span style="color: red">TBC</span> | Global Manager 2 Active     | Management |
| 22rrvnsxg03   | <span style="color: red">TBC</span> | 172.28.22.122                       | <span style="color: red">TBC</span> | Global Manager 3 Active     | Management |
| 22rrvnsxmvip  | <span style="color: red">TBC</span> | 172.28.22.123                       | <span style="color: red">TBC</span> | Local Manager VIP           | Management |
| 22rrvnsxg01   | <span style="color: red">TBC</span> | 172.28.22.124                       | <span style="color: red">TBC</span> | Local Manager 1             | Management |
| 22rrvnsxg02   | <span style="color: red">TBC</span> | 172.28.22.125                       | <span style="color: red">TBC</span> | Local Manager 2             | Management |
| 22rrvnsxg03   | <span style="color: red">TBC</span> | 172.28.22.126                       | <span style="color: red">TBC</span> | Local Manager 3             | Management |
| 22rrvnsxedg01 | <span style="color: red">TBC</span> | 172.28.22.202                       | <span style="color: red">TBC</span> | Prod Edge node 1            | Compute    |
| 22rrvnsxedg02 | <span style="color: red">TBC</span> | 172.28.22.206                       | <span style="color: red">TBC</span> | Prod Edge node 2            | Compute    |
| 22rrvnsxedg20 | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | Stretched Edge Node 1       | Compute    |
| 22rrvnsxedg04 | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | Stretched Edge Node 2       | Compute    |

### East Burwood

| Name               | VLAN                                | IP Addresses                        | VRF                                 | Description           | CLUSTER                             |
| ------------------ | ----------------------------------- | ----------------------------------- | ----------------------------------- | --------------------- | ----------------------------------- |
| o3ebvnsxmvip       | <span style="color: red">TBC</span> | 172.19.175.101                      | <span style="color: red">TBC</span> | Local Manager VIP     | <span style="color: red">TBC</span> |
| o3ebvnsxm02        | <span style="color: red">TBC</span> | 172.19.175.102                      | <span style="color: red">TBC</span> | Local Manager 1       | <span style="color: red">TBC</span> |
| o3ebvnsxm02        | <span style="color: red">TBC</span> | 172.19.175.103                      | <span style="color: red">TBC</span> | Local Manager 2       | <span style="color: red">TBC</span> |
| o3ebvnsxm03        | <span style="color: red">TBC</span> | 172.19.175.104                      | <span style="color: red">TBC</span> | Local Manager 3       | <span style="color: red">TBC</span> |
| o3ebvnsxedg01      | <span style="color: red">TBC</span> | 172.28.23.250                       | <span style="color: red">TBC</span> | Prod Edge node 1      | <span style="color: red">TBC</span> |
| o3ebvnsxedg02      | <span style="color: red">TBC</span> | 172.28.23.251                       | <span style="color: red">TBC</span> | Prod Edge node 2      | <span style="color: red">TBC</span> |
| o3ebvnsxedg10      | <span style="color: red">TBC</span> | 172.28.23.252                       | <span style="color: red">TBC</span> | DMZ Edge node 1       | <span style="color: red">TBC</span> |
| o3ebvnsxedg11      | <span style="color: red">TBC</span> | 172.28.23.253                       | <span style="color: red">TBC</span> | DMZ Edge node 2       | <span style="color: red">TBC</span> |
| o3ebvnsxedg22      | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | Stretched Edge Node 1 | <span style="color: red">TBC</span> |
| o3ebvnsxedg21      | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | Stretched Edge Node 1 | <span style="color: red">TBC</span> |
| <center>-</center> | 29                                  | <span style="color: red">TBC</span> | <center>-</center>                  | RTEP VLAN             | <center>-</center>                  |

---

## NPTE

The lab was the original location for NSX-T to be installed and has a couple of differences to Production.  These are as follows:

* Lab uses N-VDS instead of the newer VDS
* MTU cannot be increased beyond 1500 as this will require the N-VDS to be recreated



![image-20230913180257638](/assets/img/optus-handbook/image-20230913180257638.png)

### Rosebury

| Name          | VLAN                                | IP Addresses                        | VRF                                 | Description     | CLUSTER    |
| ------------- | ----------------------------------- | ----------------------------------- | ----------------------------------- | --------------- | ---------- |
| o2rovnsxmgr01 | 33                                  | 172.19.175.101                      | M348:T2:DC2-NPTE-INFRA1             | NSX Manager     | Management |
| o2rovnsxedg01 | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | NSX Edge Node 1 | Compute    |
| o2ronsxedg02  | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | <span style="color: red">TBC</span> | NSX Edge Node 2 | Compute    |



---

# NSX Processes

# RTAP

![RTAP](/assets/img/optus-handbook/RTAP.png)



RTAP utilises two voice routers located in Mascot and Blacktown. These are then presented to VMWare on VL2048. This does not route via NSX but is presented to VMWare as a standard port-group on the vDS. 



[^fn1]: NPTE uses VMware NSX-T to manage bgp peering of networks and vrfs

[^fn2]: This is currently listed as requiring setup.
[^fn3]: NAC environement details TBC once available.
[^fn4]: This vlan conflicts with NSX-T and must be reallocated or we move RTEPs to a different VLAN ID
[^fn5]: While routing iSCSI traffic is technically possible, each routing hop adds latency to the storage traffic and load onto the network routing infrastructure. It is a common recommendation to use non-routed networks for iSCSI traffic. For this reason, HPE cannot guarantee the I/O performance for VMs using the cross-site routed iSCSI networks.

​	
