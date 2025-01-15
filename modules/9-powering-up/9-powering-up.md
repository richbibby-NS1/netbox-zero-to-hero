# Module 9 -  Powering Up!

# Introduction

Hello and welcome to module 9 of the NetBox 'Zero-to-Hero' course. In [Module 8: What About Virtualization?](../8-what-about-virtualization/8-what-about-virtualization.md), Network Engineer Susan added the required physical servers for the vSphere cluster, created the cluster and added the Virtual Machines. She also defined the services that are running on the VM servers.

In this video, Eric will add the facility power panels and feeds for the new Brisbane branch office, and then also add the PDUs and power cable connections, so all the devices in the new communications cabinet can be powered on.

By the end of this module you will be able to:
- Describe how NetBox models facility power as discrete power panels and feeds.
- Understand how to add Power Distribution Units (PDUs) to supply power to individual devices
- Use the web interface to manually add all data relating to facility power including bulk uploading cable connections from CSV data

## Get Hands On
If you'd like to follow along with the examples used in this course, it's super easy to do, and you have a few options:
1. Sign up for the NetBox Cloud [FREE Plan](https://netboxlabs.com/free-netbox-cloud/) - All the power and flexibility of NetBox - with the security, reliability, and management covered by NetBox Labs - for free. Select the option for no data to ensure you start with an empty NetBox database.
2. Run NetBox as a container with [NetBox Docker](https://github.com/netbox-community/netbox-docker)
3. Follow the [official documentation](https://netboxlabs.com/docs/netbox/en/stable/installation/) and do a full installation of all the NetBox components. These instructions have been tested on Ubuntu and CentOS Linux.
4. Use the public [demo instance](https://demo.netbox.dev/) of NetBox

The software versions used in the video for this module are:
- `NetBox `v4.1.11``

## Power Tracking In NetBox

From the [official NetBox docs](https://netboxlabs.com/docs/netbox/en/stable/features/power-tracking/)
>### Power Tracking
>As part of its DCIM feature set, NetBox supports modeling facility power as discrete power panels and feeds. These are most commonly used to document power distribution within a data center, but can serve more traditional environments as well.
>
>### Power Panels
>A power panel is the furthest upstream power element modeled in NetBox. It typically represents a power distribution panel (or breaker panel) where facility power is split into multiple discrete circuits, which are modeled as feeds.
>
>Each power panel is associated with a site, and may optionally be associated with a particular location within that site. There is no limit to how many power feeds a single panel can supply, however both of these object types should map to real-world objects.
>
>### Power Feeds
>A power feed represents a discrete power circuit originating from an upstream power panel. Each power feed can be assigned a name, operational status, and various electrical characteristics such as supply (AC or DC), voltage, amperage, and so on.
>
>A device power port can be connected to a power feed via a cable. Only one port can be connected to a feed: Where multiple devices draw power from the same feed, a power distribution unit (PDU) must be modeled as an individual device mapping a power port to multiple power outlets to which the downstream devices can connect
>
>Each power feed in NetBox is assigned a type: primary or redundant. This allows easily modeling redundant power distribution topologies. In scenarios involving only a single, non-redundant power supply, mark all power feeds as primary.

## The Project - Adding the Facility Power Panels, Feeds and PDUs
Eric has designed the following solution for power in the new Brisbane Comms Room:

### Power Panels
There are two power panels provided by the facilities management company:

| Panel Name | Location |
| --- | --- |
| AUBRI01-PWR-PAN-1 | Comms Room |
| AUBRI01-PWR-PAN-2 | Comms Room |

### Power Feeds
Each power panel supplies a single power feed (discrete circuit) to the Comms room rack as follows:

| Feed Name | Power Panel | Rack | Type | Supply | Voltage | Amps | Phase |
| --- | --- | --- | --- | :---: | :---: | :---: | :---: |
| AUBRI01-PWR-FEED-1 | AUBRI01-PWR-PAN-1 | AUBRI01-RK-01 | Primary | AC | 200 | 16 | 3-phase |
| AUBRI01-PWR-FEED-2 | AUBRI01-PWR-PAN-2 | AUBRI01-RK-01 | Redundant | AC | 200 | 16 | 3-phase |

### Power Distribution Units (PDUs)
Two APC (model AP7921B - 16A, 208/230V) PDU's will be installed in the rack and will connect to the power feeds via a single power port, and each PDU has eight C13 power outlets that can be connected to device power supplies:

| PDU Name | Rack Location |
| --- | :---: |
| AUBRI01-PDU-1 | 11 (rear) |
| AUBRI01-PDU-2 | 12 (rear) |

You can find the YAML definition for this device in the [Device Type Library repo](https://github.com/netbox-community/devicetype-library/blob/master/device-types/APC/AP7921B.yaml).

### Device to PDU Power Connections
The following cables will be added for power connections between the devices and the PDUs:

| Device | Power Port | PDU | Power Outlet |
| --- | :---: | --- | :---: |
| AUBRI01-SW-1 | PSU0 | AUBRI01-PDU-1 | Outlet 1 |
| AUBRI01-SW-1 | PSU1 | AUBRI01-PDU-2 | Outlet 1 |
| AUBRI01-CON-1 | ps1 | AUBRI01-PDU-1 | Outlet 2 |
| AUBRI01-RTR-1 | PSU0 | AUBRI01-PDU-2 | Outlet 2|
| AUBRI01-VSP-1 | PSU1 | AUBRI01-PDU-1 | Outlet 3|
| AUBRI01-VSP-1 | PSU2 | AUBRI01-PDU-2 | Outlet 3|
| AUBRI01-VSP-2 | PSU1 | AUBRI01-PDU-1 | Outlet 4|
| AUBRI01-VSP-2 | PSU2 | AUBRI01-PDU-2 | Outlet 4|

### PDU to Power Feed and Panel Connections
The following cables will be added for power connections between the PDUs and the power feeds:

| PDU | Power Port | Power Feed | Power Panel |
| --- | :---: | --- | --- |
| AUBRI01-PDU-1 | 1 | AUBRI01-PWR-FEED-1 | AUBRI01-PWR-PAN-1 |
| AUBRI01-PDU-2 | 1 | AUBRI01-PWR-FEED-2 | AUBRI01-PWR-PAN-2 |

### Device Power Ports - Maximum and Allocated Draw
In order to track the power that each of the devices in the rack is consuming, NetBox allows you to model both the Maximum and Allocated Draw, and by doing this for each of the Power Ports on the racked equipment you can track power utilization.

The [docs](https://netboxlabs.com/docs/netbox/en/stable/models/dcim/powerport/#maximum-draw) define these fields in the data model as follows:
>**Maximum Draw** - The maximum amount of power this port consumes (in watts).
>
>**Allocated Draw** - The budgeted amount of power this port consumes (in watts).

All of the device types that have been added to NetBox for the Brisbane project have their Power Port Maximum Draw values already defined in the device type definitions, and Eric has also defined their Allocated Draw values as follows:

| Device | Power Port | Maximum Draw | Allocated Draw |
| --- | :---: | :---: | :---: |
| AUBRI01-CON-1 | ps1 | 35 | 20 |
| AUBRI01-RTR-1 | PSU0 | 125 | 100 |
| AUBRI01-SW-1 | PSU0 | 1100 | 700 |
| AUBRI01-SW-1 | PSU1 | 1100 | 700 |
| AUBRI01-VSP-1 | PSU1 | 1400 | 900 |
| AUBRI01-VSP-1 | PSU2 | 1400 | 900 |
| AUBRI01-VSP-2 | PSU1 | 1400 | 900 |
| AUBRI01-VSP-2 | PSU2 | 1400 | 900 |

## Video - Adding The Facility Power Panels, Feeds and PDUs
OK, so that's the planning work done - let's make this happen in NetBox!! This video will step you through the whole process from adding the power panels, feeds and PDUs, through to connecting the power cables to the network devices and servers.

If you are following along you can find the [CSV data](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/modules/9-powering-up/csv_data) for the PDU devices and the power cable connections in the course GitHub repository.

[![Adding the facility power panels, feeds and PDUs](https://img.youtube.com/vi/Ky7UXDPTobA/maxresdefault.jpg)](https://www.youtube.com/watch?v=Ky7UXDPTobA)

## Summary
In this module you have learned how NetBox models facility power as discrete power panels and feeds. You also learned how to add Power Distribution Units (PDUs) to supply power to individual devices.

In [Module 10: Providers and Circuits](../10-providers-and-circuits/10-providers-and-circuits.md) you will learn how NetBox models providers and circuits, and how to "connect" circuits directly to device interfaces via cables. You will also add to your Postman collection for NetBox, with some new API calls to add and verify this data programmatically.

## Join the Discussion
If you have any questions as you go through the course then pop on over to the [NetBox Learning Channel](https://netdev-community.slack.com/archives/C0453L6565C) on the NetDev Community Slack! If you aren't already a member then you can sign up for free [here](https://netdev.chat/).

## Useful Links
- [NetBox Cloud](https://netboxlabs.com/free-netbox-cloud/) is a hosted solution offered by NetBox Labs, and you can easily get started with the Free Plan.
- [Zero To Hero Git Repo](https://github.com/netbox-community/netbox-zero-to-hero)
- [Official NetBox Documentation](https://netboxlabs.com/docs/netbox/en/stable/)
- [NetBox Docker](https://github.com/netbox-community/netbox-docker)
