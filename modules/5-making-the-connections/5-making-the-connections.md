# Module 5 - Making The Connections
# Introduction

Hello and welcome to module 5 of the NetBox 'Zero-to-Hero' course. In [Module 4: IP Addressing and VLANs](../4-ip-addressing-and-vlans/4-ip-addressing-and-vlans.md), Susan used the [Ansible Galaxy Collection for NetBox](https://galaxy.ansible.com/netbox/netbox) to populate NetBox with the IP addressing and VLAN data for the new Brisbane branch office.

In this module, Eric will be adding the cables and connections required to connect the devices in the new Brisbane branch office network together.

By the end of this module you will be able to:
- Describe how NetBox models Cables and Connections
- Use the web interface to bulk upload data for Cables and Connections from a CSV file
- Use the web interface to view the Cables, Interface and Console connections

## Get Hands On
If you'd like to follow along with the examples used in this course, it's super easy to do, and you have a few options:
1. Sign up for the NetBox Cloud [FREE Plan](https://netboxlabs.com/free-netbox-cloud/) - All the power and flexibility of NetBox - with the security, reliability, and management covered by NetBox Labs - for free. Select the option for no data to ensure you start with an empty NetBox database.
2. Run NetBox as a container with [NetBox Docker](https://github.com/netbox-community/netbox-docker)
3. Follow the [official documentation](https://netboxlabs.com/docs/netbox/en/stable/installation/) and do a full installation of all the NetBox components. These instructions have been tested on Ubuntu and CentOS Linux.
4. Use the public [demo instance](https://demo.netbox.dev/) of NetBox

The software versions used in the video for this module are:
- `NetBox v4.1.8`

## Modelling Cables and Connections in NetBox

From the [docs](https://netboxlabs.com/docs/netbox/en/stable/features/devices-cabling/#cables)
>NetBox models cables as connections among certain types of device components and other objects. Each cable can be assigned a type, color, length, and label. NetBox will enforce basic sanity checks to prevent invalid connections. (For example, a network interface cannot be connected to a power outlet.)
>
>Either end of a cable may terminate to multiple objects of the same type. For example, a network interface can be connected via a fiber optic cable to two discrete ports on a patch panel (each port attaching to an individual fiber strand in the patch cable).

## The Project - New Brisbane Site Cables, Interface and Console Connections
Eric has planned the following cabling schedule for the new Brisbane site, and this needs to be added to NetBox. The ISP connection cables will be added in a later module.

### Device to Device Interface connections
These cables will interconnect network devices over Ethernet interfaces. For example a direct connection from the WAN Router to the Access Switch.

| Device A | Interface A | Device B | Interface B | Cable Type | Cable Color | Cable Length
| --- | --- | --- | --- | :---: | :---: | :---: |
| AUBRI01-RTR-1 | GigabitEthernet0 | AUBRI01-SW-1 | ge-0/0/47 | CAT6 | Red | 0.5M |
| AUBRI01-RTR-1 | GigabitEthernet0/0/0 | AUBRI01-SW-1 | ge-0/0/0 | CAT6 | Red | 0.5M |
| AUBRI01-CON-1 | Ethernet | AUBRI01-SW-1 | ge-0/0/46 | CAT6 | Red | 0.5M |

### Device to Console Server connections
These cables will connect the console port on WAN Router and Switch to Console server ports. These connections will be used for remote out-of-band access to the devices should it be required.

| Device A | Interface A | Device B | Interface B | Cable Type | Cable Color | Cable Length
| --- | --- | --- | --- | :---: | :---: | :---: |
| AUBRI01-RTR-1 | con 0 | AUBRI01-CON-1 | ttyS1 | CAT6 | Blue | 1M |
| AUBRI01-SW-1 | Console | AUBRI01-CON-1 | ttyS2 | CAT6 | Blue | 1M |

### Switch to Patch Panel Connections
These cables will connect 18 Access Switch Ethernet interfaces to the front ports of the UTP Patch Panel. The rear ports of the patch panel will have the CAT6 structured cabling for the office space terminated onto them, and they will connect out to the data outlets in the office (the rear port to outlet cables will not be modelled in the demo, but the process to do so is identical). The Wireless Access Points will be connected to data outlets in the office space.

| Device A | Interface A | Device B | Interface B | Cable Type | Cable Color | Cable Length
| --- | --- | --- | --- | :---: | :---: | :---: |
| AUBRI01-SW-1 | ge-0/0/10 - 27 | AUBRI1-PAN-1 | 01 - 18 | CAT6 | Purple | 0.5M

## Video - Adding Cables and Connections into NetBox
OK, so that's the planning and design work done - now onto the demo! This video will step you through how to populate NetBox with Cables, Interface and Console connections, and then to view that data in the Web Interface. As always the best way to understand the power of NetBox is to dive right in, so let's get started!

If you are following along you can find the [CSV data](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/modules/5-making-the-connections/csv_data) in the course Git Repository.

[![Adding cables and connections into Netbox](https://img.youtube.com/vi/FTjqGPS2oSo/maxresdefault.jpg)](https://www.youtube.com/watch?v=FTjqGPS2oSo)

## Summary
In this module you have learned how NetBox models Cables, Interface and Console connections, and how to bulk upload them via the web interface.

In [Module 6: Setting up the WiFi](../6-Setting-up-the-WiFi/6-Setting-up-the-WiFi.md), you will learn how to add the Wireless LANs for the new Brisbane office, using simple Python scripts to interact with the NetBox REST API.

## Join the Discussion
If you have any questions as you go through the course then pop on over to the [NetBox Learning Channel](https://netdev-community.slack.com/archives/C0453L6565C) on the NetDev Community Slack! If you aren't already a member then you can sign up for free [here](https://netdev.chat/).

## Useful Links
- [NetBox Cloud](https://netboxlabs.com/free-netbox-cloud/) is a hosted solution offered by NetBox Labs, and you can easily get started with the Free Plan.
- [Official NetBox Documentation](https://netboxlabs.com/docs/netbox/en/stable/)
- [NetBox Docker](https://github.com/netbox-community/netbox-docker)
