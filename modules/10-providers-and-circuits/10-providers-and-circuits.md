# Module 10 -  Providers and Circuits

# Introduction

Hello and welcome to module 10 of the NetBox 'Zero-to-Hero' course. In [Module 9: Powering Up!](../9-powering-up/9-powering-up.md), Network Engineer Eric added the facility power panels and feeds for the new Brisbane branch office, along with the PDUs and power cable connections, so all the devices in the new communications cabinet can be powered on.

Now it's time to connect the new site to the Internet, and in this module Network Engineer Susan will add the new Internet Circuit.

By the end of this module you will be able to:
- Describe how NetBox models service providers and circuits
- Understand how to "connect" circuits directly to device interfaces via cables
- Use Postman to make API calls to NetBox to add this data programmatically

## Get Hands On
If you'd like to follow along with the examples used in this course, it's super easy to do, and you have a few options:
1. Sign up for the NetBox Cloud [FREE Plan](https://netboxlabs.com/free-netbox-cloud/) - All the power and flexibility of NetBox - with the security, reliability, and management covered by NetBox Labs - for free. Select the option for no data to ensure you start with an empty NetBox database.
2. Run NetBox as a container with [NetBox Docker](https://github.com/netbox-community/netbox-docker)
3. Follow the [official documentation](https://netboxlabs.com/docs/netbox/en/stable/installation/) and do a full installation of all the NetBox components. These instructions have been tested on Ubuntu and CentOS Linux.
4. Use the public [demo instance](https://demo.netbox.dev/) of NetBox

The NetBox version used in the video for this module is `v4.1.11`, and the following course materials used in the demo are available:
- [Postman Collection](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/postman) for making API calls

## Managing Providers and Circuits in NetBox

From the [official NetBox docs](https://netboxlabs.com/docs/netbox/en/stable/features/circuits/)
>### Circuits
>NetBox is ideal for managing your network's transit and peering providers and circuits. It provides all the flexibility needed to model physical circuits in both data center and enterprise environments, and allows for "connecting" circuits directly to device interfaces via cables.
>
>### Providers
>A provider is any organization which provides Internet or private connectivity. Typically, these are large carriers, however they might also include regional providers or even internal services. Each provider can be assigned account and contact details, and may have one or more AS numbers assigned to it.
>
>Sometimes you'll need to model provider networks into which you don't have full visibility; these are typically represented on topology diagrams with cloud icons. NetBox facilitates this through its provider network model: A provider network represents a "black box" network to which your circuits can connect. A common example is a provider MPLS network connecting multiple sites.
>
>### Circuits
>A circuit is a physical connection between two points, which is installed and maintained by an external provider. For example, an Internet connection delivered as a fiber optic cable would be modeled as a circuit in NetBox.
>
>Each circuit is associated with a provider and assigned a circuit ID, which must be unique to that provider. A circuit is also assigned a user-defined type, operational status, and various other operating characteristics.
>
>Each circuit may have up to two terminations (A and Z) defined. Each termination can be associated with a particular site or provider network. In the case of the former, a cable can be connected between the circuit termination and a device component to map its physical connectivity.

## The Project - Adding the Internet Circuit
The new Brisbane branch office will have a single Internet Access circuit from an ISP (Internet Service Provider). The circuit will be fiber based with an Ethernet hand-off that connects directly into an interface on the new Brisbane Router, over a Cat6 copper cable.

The ISP has provisioned the new circuit for as follows:

| Provider | Circuit ID | Circuit Type | Bandwidth (Mbps) | Termination Device | Termination Interface |
| --- | --- | --- | :---: | --- | --- |
| Telstra| TelCir12345 | Internet Access | 200 | AUBRI01-RTR-01 | GigabitEthernet0/0/1 |

## Video - Adding the Provider and the Internet Circuit
OK, so that's the planning work done - let's get to the fun stuff!! This video will step you through the whole process from adding the provider and circuit, through to connecting the circuit to the Router interface.

If you are following along, don't forget to use the [Postman Collection](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/postman) for making the API calls.

[![Adding the provider and the internet circuit](https://img.youtube.com/vi/GgnBzAYgZGY/maxresdefault.jpg)](https://www.youtube.com/watch?v=GgnBzAYgZGY)

## Summary
In this module you have learned how NetBox models providers and circuits, and how to "connect" circuits directly to device interfaces via cables. You also added to your Postman collection for NetBox, to make API calls to add this data programmatically.

In [Module 11 - Custom Scripts](../11-custom-scripts/11-custom-scripts.md) you will learn what Custom Scripts are in NetBox and what kind of tasks they can be used to accomplish. You'll also learn the basics of writing Custom Scripts and also where to find documentation to help you develop your own scripts. Plus you can kick start your own Custom Scripts collection by copying the two example scripts included with the module to get you up and running!

## Join the Discussion
If you have any questions as you go through the course then pop on over to the [NetBox Learning Channel](https://netdev-community.slack.com/archives/C0453L6565C) on the NetDev Community Slack! If you aren't already a member then you can sign up for free [here](https://netdev.chat/).

## Useful Links
- [NetBox Cloud](https://netboxlabs.com/free-netbox-cloud/) is a hosted solution offered by NetBox Labs, and you can easily get started with the Free Plan.
- [Zero To Hero Git Repo](https://github.com/netbox-community/netbox-zero-to-hero)
- [Official NetBox Documentation](https://netboxlabs.com/docs/netbox/en/stable/)
- [NetBox Docker](https://github.com/netbox-community/netbox-docker)
