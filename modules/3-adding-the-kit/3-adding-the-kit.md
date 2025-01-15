# Module 3 - Adding the Kit
# Introduction

Hello and welcome to module 3 of the NetBox 'Zero-to-Hero' course. In [Module 2: Setting up the Organization](../2-setting-up-the-organization/2-setting-up-the-organization.md) you learned how to model an organization within NetBox, and how to use the Web Interface to both manually create individual objects, and bulk import objects using CSV-formatted data.

In this module we will continue to populate NetBox with data for our fictional organization. Our intrepid Network Engineer Eric will be adding the network devices that are going to be installed at the planned new Brisbane branch office, making use of the NetBox REST API (he's heard about these API things on the 'Router Nerds' podcast and is keen to learn how to use one).

By the end of this module you will be able to:
- Describe how NetBox models devices
- Use Postman to make REST API calls to NetBox
- Save time by making use of the community library of pre-defined device types

## Why Use The REST API?
You can obviously add all this data manually via the Web Interface, but by getting hands on with the REST API, you will start to take your skills with NetBox to the next level! A lot of Network Engineers have never used an API (they have surely heard of them by now though) and they might be unsure of how to work with one, or why they should even bother.

The goal of this module is to demystify REST API's and show you how powerful and simple to use the NetBox REST API is. You will quickly start to see how you can interact programmatically with NetBox and how NetBox can drive your Network Automation efforts going forward.

## Get Hands On
If you'd like to follow along with the examples used in this course, it's super easy to do, and you have a few options:
1. Sign up for the NetBox Cloud [FREE Plan](https://netboxlabs.com/free-netbox-cloud/) - All the power and flexibility of NetBox - with the security, reliability, and management covered by NetBox Labs - for free. Select the option for no data to ensure you start with an empty NetBox database.
2. Run NetBox as a container with [NetBox Docker](https://github.com/netbox-community/netbox-docker)
3. Follow the [official documentation](https://netboxlabs.com/docs/netbox/en/stable/installation/) and do a full installation of all the NetBox components. These instructions have been tested on Ubuntu and CentOS Linux.
4. Use the public [demo instance](https://demo.netbox.dev/) of NetBox

The NetBox version used in the video for this module is `v4.1.11`, and the following course materials used in the demo are available:
- [Postman Collection](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/postman) for making API calls
- [YAML files](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/modules/3-adding-the-kit/yaml_data) used for device types

## Devices
Devices are a key part of NetBox - after all NetBox is a tool for modeling network infrastructure. A device can be any piece of physical hardware installed within your network, such as a server, router, or switch, and may optionally be mounted within a rack. Within each device, resources such as network interfaces and console ports are modeled as discrete components, which may optionally be grouped into modules.

Device objects in NetBox have some dependencies that must already exist in NetBox before you can add the device. They are Manufacturer (the organization that produces the hardware) and Device Type. A device can be installed at a particular position within an equipment rack, or simply associated with a site (and optionally with a location within that site).

## Device Types
A device type is a combination of manufacturer and hardware model that maps to actual devices that exist in the real world. Device types typically have a number of components modelled on them too for things like interfaces, console ports, and power ports etc.

An example of a device type is a Cisco ISR4321 Router (part_number: ISR4321/K9), and this might have the following component templates defined:

- 3 x GigabitEthernet interfaces
- 1 x Console Port
- 1 x Power Port

Once a device type has been created, then each new device instance of that particular device type you add will have all of the components defined in the device type.

## Device Roles
These are the functional roles of devices, and are fully customizable by the user. For example, you might create roles for core switches, distribution switches, and access switches within your network.

## Platforms
A platform defines the type of software running on a device or virtual machine, for example Cisco IOS version 15. NetBox can optionally connect to network devices (to retrieve device information such as LLDP Neighbors) using [NAPALM](https://napalm.readthedocs.io/en/latest/) and you can also (optionally) define the driver to be used (eg. ios) as part of the Platform definition.

## The Project - New Branch Site Devices
Our fictional organization has standardized on the following network devices for their Branch office network deployments:

| Manufacturer | Device Type | Device Role | Platform |
| :--- | :--- | :--- | :--- |
| Cisco | ISR4321 | WAN Router | Cisco IOS |
| Juniper | EX4300-48P | Access Switch | Juniper JunOS |
| Cisco Meraki | MR56 | Wireless AP | n/a |
| Avocent | ACS16 | Console Server | n/a |
| Panduit | Mini-Com High Density Patch Panel (48 Port, 1RU) | Patch Panel | n/a |

## Video - Adding Devices Into NetBox
The video demo will now show you how to add all of the device data into NetBox mainly using the REST API (the device types will be uploaded manually as YAML files). As always the best way to understand the power of NetBox is to dive right in, so let's get started!

If you are following along, don't forget to use the [Postman Collection](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/postman) for making the API calls and the [YAML files](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/modules/3-adding-the-kit/yaml_data) used for adding device types.


[![Adding devices into Netbox](https://img.youtube.com/vi/dA3LZiV7UIg/maxresdefault.jpg)](https://www.youtube.com/watch?v=dA3LZiV7UIg)

## Summary
In this module you learned how to add devices into NetBox, making use of the NetBox REST API and a Postman collection.

In [Module 4: IP Addressing and VLANs](../4-ip-addressing-and-vlans/4-ip-addressing-and-vlans.md),  Susan will use the [Ansible Galaxy Collection for NetBox](https://galaxy.ansible.com/netbox/netbox) to populate NetBox with the IP addressing and VLAN data for the new Brisbane branch office.

## Join the Discussion
If you have any questions as you go through the course then pop on over to the [NetBox Learning Channel](https://netdev-community.slack.com/archives/C0453L6565C) on the NetDev Community Slack! If you aren't already a member then you can sign up for free [here](https://netdev.chat/).

## Useful Links
- [NetBox Cloud](https://netboxlabs.com/free-netbox-cloud/) is a hosted solution offered by NetBox Labs, and you can easily get started with the Free Plan.
- [Official NetBox Documentation](https://netboxlabs.com/docs/netbox/en/stable/)
- [NetBox Community Device Type Library](https://github.com/netbox-community/devicetype-library)
- [NetBox Docker](https://github.com/netbox-community/netbox-docker)
