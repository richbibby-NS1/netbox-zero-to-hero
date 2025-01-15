# Module 8 - What About Virtualization?

# Introduction

Hello and welcome to module 8 of the NetBox 'Zero-to-Hero' course. In [Module 7: Automate All the Things!](../7-automate-all-the-things/7-automate-all-the-things.md), Eric used Ansible to extract data from NetBox and then use that data to automate the creation of basic device configurations for the WAN Router (Cisco IOS) and the Access Switch (Juniper JunOS), at the new Brisbane branch office.

The IT Manager has now decided that as there are going to be two data scientists working out of the new Brisbane office, it makes sense to have a Database server located on premises there, along with a local file and print server. These servers will be virtual machines hosted on a VMware vSphere cluster.

In this video, Network Engineer Susan will add the required physical servers for the vSphere cluster, create the cluster and add the Virtual Machines for the servers. She will also define the services including the protocol and port numbers that will be running on the VM servers, and all of this will be done via the web interface.

By the end of this module you will be able to:
- Describe how NetBox models Virtualization, including Cluster Types, Clusters, Platforms, VM's and VM Interfaces
- Describe how to model network services associated with devices and/or virtual machines, along with specific IP addresses
- Use the web interface to manually add Virtualization and service data, including bulk uploading larger amounts of data where required

## Get Hands On
If you'd like to follow along with the examples used in this course, it's super easy to do, and you have a few options:
1. Sign up for the NetBox Cloud [FREE Plan](https://netboxlabs.com/free-netbox-cloud/) - All the power and flexibility of NetBox - with the security, reliability, and management covered by NetBox Labs - for free. Select the option for no data to ensure you start with an empty NetBox database.
2. Run NetBox as a container with [NetBox Docker](https://github.com/netbox-community/netbox-docker)
3. Follow the [official documentation](https://netboxlabs.com/docs/netbox/en/stable/installation/) and do a full installation of all the NetBox components. These instructions have been tested on Ubuntu and CentOS Linux.
4. Use the public [demo instance](https://demo.netbox.dev/) of NetBox

The software versions used in the video for this module are:
- `NetBox v4.1.11`

## Virtualization In NetBox

From the [docs](https://netboxlabs.com/docs/netbox/en/stable/features/virtualization/)
>### Virtualization
>Virtual machines and clusters can be modeled in NetBox alongside physical infrastructure. IP addresses and other resources are assigned to these objects just like physical objects, providing a seamless integration between physical and virtual networks.
>
>### Clusters
>A cluster is one or more physical host devices on which virtual machines can run. Each cluster must have a type and operational status, and may be assigned to a group. (Both types and groups are user-defined.) Each cluster may designate one or more devices as hosts, however this is optional.
>
>### Virtual Machines
>A virtual machine is a virtualized compute instance. These behave in NetBox very similarly to device objects, but without any physical attributes. For example, a VM may have interfaces assigned to it with IP addresses and VLANs, however its interfaces cannot be connected via cables (because they are virtual). Each VM may also define its compute, memory, and storage resources as well.

## Service Mapping In NetBox

From the [docs](https://netboxlabs.com/docs/netbox/en/stable/features/ipam/#:~:text=an%20authoritative%20RIR.-,Service%20Mapping,-NetBox%20models%20network)
>NetBox models network applications as discrete service objects associated with devices and/or virtual machines, and optionally with specific IP addresses attached to those parent objects. These can be used to catalog the applications running on your network for reference by other objects or integrated tools.
>
>To model services in NetBox, begin by creating a service template defining the name, protocol, and port number(s) on which the service listens. This template can then be easily instantiated to "attach" new services to a device or virtual machine.

## The Project - Adding The Virtualization Cluster and Virtual Machines
Susan has designed the following solution for the new Virtual Servers in Brisbane:

### VM Cluster

| Cluster Name | Cluster Type | Devices | Device Type |
| --- | --- | --- | ---|
| AUBRI01-VSPHERE-1 | VMware vSphere | AUBRI01-VSP-1, AUBRI01-VSP-2 | HPE ProLiant DL380 Gen9 |

### Device Interfaces

| Device | Interface | 802.1Q Mode | LAG Interface |
| --- | --- | --- | --- |
| AUBRI01-VSP-1 | iLO | Access (vlan 50) | N/A |
| AUBRI01-VSP-1 | Gig-E 1 | Tagged (all) | ae0 |
| AUBRI01-VSP-1 | Gig-E 2 | Tagged (all) | ae0 |
| AUBRI01-VSP-2 | iLO | Access (vlan 50) | N/A |
| AUBRI01-VSP-2 | Gig-E 1 | Tagged (all) | ae1 |
| AUBRI01-VSP-2 | Gig-E 2 | Tagged (all) | ae1 |

### Rack Positions
The Physical servers for the cluster will be mounted in the Brisbane rack (AUBRI01-RK-01) as follows:

| Device | Rack Location (RUs) |
| --- | --- |
| AUBRI01-VSP-1 | 1-2 |
| AUBRI01-VSP-2 | 3-4 |

### Server to Switch Connections
These cables will connect the physical servers to the local access switch:

| Device A | Interface A | Device B | Interface B | Cable Type | Cable Color | Cable Length |
| --- | --- | --- | --- | :---: | :---: | :---: |
| AUBRI01-VSP-1 | iLO | AUBRI01-SW-1 | ge-0/0/44 | CAT6 | Blue | 2M |
| AUBRI01-VSP-1 | Gig-E 1 | AUBRI01-SW-1 | ge-0/0/1 | CAT6 | Green | 2M |
| AUBRI01-VSP-1 | Gig-E 2 | AUBRI01-SW-1 | ge-0/0/2 | CAT6 | Green | 2M |
| AUBRI01-VSP-2 | iLO | AUBRI01-SW-1 | ge-0/0/45 | CAT6 | Blue | 2M |
| AUBRI01-VSP-2 | Gig-E 1 | AUBRI01-SW-1 | ge-0/0/3 | CAT6 | Green | 2M |
| AUBRI01-VSP-2 | Gig-E 2 | AUBRI01-SW-1 | ge-0/0/4 | CAT6 | Green | 2M |

### Virtual Machines
The VMs are specified as follows:

| Name | Platform | Memory | Disk | CPUs | Services | VM Interface
| --- | --- | --- | --- | --- | --- | --- |
| AUBRI01-SQL-01 | Ubuntu 22.10 | 128 GB | 200 GB | 32 | tcp/5432, tcp/22 | eth0 (tagged vlan 10) |
| AUBRI01-WIN-01 | Windows Server 2022 | 64 GB | 128 GB | 8 | tcp/139, tcp/445 | eth0 (tagged vlan 10) |


## Video - Adding Virtualization Clusters and Virtual Machines
OK, so that's the planning work done - let's get to the fun stuff!! This video will step you through the whole process from adding the required physical servers for the VMware vSphere cluster, creating the cluster, through to adding the Virtual Machines and the application services.

If you are following along you can find the [CSV data](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/modules/8-what-about-virtualization/csv_data) for the new devices and cables in the course GitHub repository.

[![Adding virtualization clusters and virtual machines](https://img.youtube.com/vi/D5iDdjZMUeo/maxresdefault.jpg)](https://www.youtube.com/watch?v=D5iDdjZMUeo)

## Summary
In this module you have learned how NetBox models Virtualization, including Cluster Types, Clusters, Platforms, VM's and VM Interfaces. You also learned how to model network services and associate them with devices or VM's, and specific IP addresses.

In [Module 9: Powering Up!](../9-powering-up/9-powering-up.md) you will learn how NetBox models facility power as discrete power panels and feeds. You will also learn how to add Power Distribution Units (PDUs) to supply power to individual devices.

## Join the Discussion
If you have any questions as you go through the course then pop on over to the [NetBox Learning Channel](https://netdev-community.slack.com/archives/C0453L6565C) on the NetDev Community Slack! If you aren't already a member then you can sign up for free [here](https://netdev.chat/).

## Useful Links
- [NetBox Cloud](https://netboxlabs.com/free-netbox-cloud/) is a hosted solution offered by NetBox Labs, and you can easily get started with the Free Plan.
- [Zero To Hero Git Repo](https://github.com/netbox-community/netbox-zero-to-hero)
- [Official NetBox Documentation](https://netboxlabs.com/docs/netbox/en/stable/)
- [NetBox Docker](https://github.com/netbox-community/netbox-docker)
