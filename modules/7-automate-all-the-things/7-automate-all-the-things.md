# Module 7- Automate All The Things!

# Introduction

Hello and welcome to module 7 of the NetBox 'Zero-to-Hero' course. In [Module 6: Setting up the WiFi](../6-Setting-up-the-WiFi/6-Setting-up-the-WiFi.md), Susan added the required Wireless LANs using some simple Python scripts to interact with the NetBox REST API. In this Module, Eric will use the Provisioning feature of NetBox to automate the automate the generation of basic device configurations for the WAN Router (Cisco IOS) and the Access Switch (Juniper JunOS), at the new Brisbane branch office.

By the end of this module you will be able to:
- Understand the basics of how Jinja templates work
- Make use of Config Contexts and Config Templates combined with data from NetBox to generate device configurations with NetBox Provisioning

## Get Hands On
If you'd like to follow along with the examples used in this course, it's super easy to do, and you have a few options:
1. Sign up for the NetBox Cloud [FREE Plan](https://netboxlabs.com/free-netbox-cloud/) - All the power and flexibility of NetBox - with the security, reliability, and management covered by NetBox Labs - for free. Select the option for no data to ensure you start with an empty NetBox database.
2. Run NetBox as a container with [NetBox Docker](https://github.com/netbox-community/netbox-docker)
3. Follow the [official documentation](https://netboxlabs.com/docs/netbox/en/stable/installation/) and do a full installation of all the NetBox components. These instructions have been tested on Ubuntu and CentOS Linux.
4. Use the public [demo instance](https://demo.netbox.dev/) of NetBox

The software versions used in the video for this module are:
- `NetBox v4.1.6`
- `Python v3.12.5`
- `ansible-core v2.13.4`
- `ansible package v6.4.0`
- `pynetbox v6.6.2`
- `netaddr v0.8.0`

## Why Use Jinja Templates For Network Device Configuration?
Using templates for network device configurations has many advantages for Network Engineers. First and foremost you get consistency - for example if you standardize your branch office networks so that you use the same device types for all of your WAN Routers and Access Switches, and the connectivity and configuration of them is the same, then the only differences between devices at different sites will be a few 'variables' such as:

- hostname
- IP addresses
- interface descriptions
- VLANs
- Routing protocol information

This means you can write a standard template for each device type, that includes all the static content that every device should have, plus variables for dynamic content that differs per device. An automation tool then simply substitutes the values for a specific device in for the 'variable' placeholders. We'll see this in action in the demo video. This means you KNOW that the configuration you have generated will be error free and consistent across all devices when it is used.

Another huge advantage of using templates is the amount of time it saves - imagine you are deploying a new data center with hundreds of switches that all use a very similar configuration - by using Jinja templates and a tool to render them you can literally generate all the device configurations in seconds. This frees up more of your time to play with that cool new NetDevOps tool you heard about on the Network Automation Nerds podcast!

It should be noted that there are other template languages around, but Jinja aligns well with other tools used in Network Automation such as Python (Jinja was built for Python) and Ansible. We are using Ansible as the automation tool in this tutorial, but you could just as easily use another too like [Nornir](https://github.com/nornir-automation/nornir) or just pure Python scripts to achieve the same result.

## A Quick Jinja Template Example
There are some great Jinja tutorials for network automation out there such as [this one](https://www.packetcoders.io/network-automation-101-hands-on-with-python-jinja-and-yaml/), but as a quick intro and example specific to NetBox, let's say we have made an API call to the endpoint `/api/dcim/devices/2/` and this returns some json output similar to this

```
{
    "id": 2,
    "url": "http://netbox/api/dcim/devices/2/",
    "display": "AUBRI01-RTR-1",
    "name": "AUBRI01-RTR-1",
    "device_type": {
        "id": 2,
        "url": "http://netbox/api/dcim/device-types/2/",
        "display": "ISR4321",
        "manufacturer": {
            "id": 1,
            "url": "http://netbox/api/dcim/manufacturers/1/",
            "display": "Cisco",
            "name": "Cisco",
            "slug": "cisco"
        },
        "model": "ISR4321",
        "slug": "isr4321"
    }
}
```
We can then use the following in a Jinja template to render a config snippet that has a variable for the device hostname:
```
{% raw %}
!
hostname {{ device.json.results[0]['name'] }}
!
{% endraw %}
```
The first word `hostname` is the static content of the config, applicable to all devices and the text between the curly braces {{ }} is the variable. The value being injected dynamically into the template variable is the value of the `name` key from the json results returned by the request. The end result of this is the final rendered configuration snippet:
```
!
hostname AUBRI01-RTR-1
!
```

Jinja templates also have powerful features like loops, logic operators, and string manipulation. Take this JunOS interface template for example:

```
{% raw %}
{%- for intf in interfaces.json.results %}
{% if 'vlan' in intf.name %}
    vlan {
    {% for ip in ip_addresses.json.results %}
      {%- if ip.assigned_object.name == intf.name %}
        unit {{ intf.name.split('.')[1] }}  {
            family inet {
                address {{ ip.address }};
            }
        }
    }
      {%- endif %}
    {% endfor %}
{% endif %}
{%- endfor %}
{% endraw %}
```
To explain whats happening here - for each interface `intf`, if the interface name `intf.name` contains the the text `vlan`, then for each of the `ip_addresses` if the `ip.assigned_object_name` is equal to the name of the interface `intf.name`, then insert the value it has for the `unit` name by splitting the `intf.name` at the dot `.` and using the 2nd element of the resulting list (lists in Python start at 0) as the value. Then finally add the value of the `ip.address` into the variable place-holder for the IPv4 address.

This results in the following configuration snippet being rendered for the interface `vlan.50`:
```
    vlan {
        unit 50  {
            family inet {
                address 192.168.2.2/26;
            }
        }
    }
```

At first glance this might seem complicated, but it's really quite straightforward and once you get your first template done the rest will be easy. You can also use the all the [templates and playbooks](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/ansible) that accompany this course to help get you started.

So, as you can see, automating network device configurations using Jinja templates is already incredibly powerful, but when you combine this with NetBox as the Source Of Truth, you are well on your way to NetDevOps Nirvana!

## Configuration Contexts
In the demo Eric will be making use of a NetBox feature called Configuration Contexts. From the [official docs](https://netboxlabs.com/docs/netbox/en/stable/features/context-data/):
>Configuration context data (or "config contexts" for short) is a powerful feature that enables users to define arbitrary data that applies to device and virtual machines based on certain characteristics. For example, suppose you want to define syslog servers for devices assigned to sites within a particular region. In NetBox, you can create a config context instance containing this data and apply it to the desired region.

The configuration context that Eric is using contains the following data, and it will apply to all devices/VMs in the Asia Pacific Region:
```
`{
    "name-servers": [
        "8.8.8.8",
        "9.9.9.9"
    ],
    "ntp-servers": [
        "192.168.4.10",
        "192.168.4.11"
    ],
    "snmp-ro-community": "R34d0nlY",
    "syslog-servers": [
        "192.168.4.12",
        "192.168.4.13"
    ]
}
```
This data will be returned in the responses to the calls made by Ansible to NetBox REST API endpoints, and can then be injected dynamically into the Jinja configuration templates, along with all the other device data pulled from NetBox.

## The Project - Automating the New Brisbane Device Configurations
Eric has created 3 Jinja [templates](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/ansible/templates/):
- `cisco-ios.j2` - for the Cisco WAN Router
- `juniper-junos.j2` - for the Juniper Access Switch
- `juniper-junos-set.j2` - for the Juniper Access Switch but using the Set command format

The data that will be pulled from NetBox to inject into the templates when they are rendered by Ansible is:
- hostname
- ip name servers
- ntp servers
- syslog servers
- snmp community and location
- interfaces
  - descriptions
  - IP addresses (if L3)
  - 802.1Q mode and VLANs (if L2)
- VLANS

**Disclaimer** *The configurations generated in this demo are not production ready by any stretch, and are purely used to illustrate how to extract the data from NetBox as your network source of truth to be consumed by an automation tool (Ansible in this case) to generate example configs. You would need to extend the templates to suit your own device types and configuration requirements, but you can certainly use them as a starting point.*

## Video - Automating Device Configurations with NetBox and Ansible
OK, so with all that said - let's get to the fun stuff!! This video will step you through the whole process from setting up NetBox as the source for the Ansible Inventory through to generating the finished configuration files.

If you are following along you can find the [Ansible playbooks](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/ansible) in the course GitHub repository and you can use these as a starting point for building your collection of playbooks for NetBox.

[![Automating device configurations with Netbox and Ansible](https://img.youtube.com/vi/aHx9EpCvi2U/maxresdefault.jpg)](https://www.youtube.com/watch?v=aHx9EpCvi2U)

## Summary
In this module you have learned how to set up Ansible to use NetBox as the source of truth for it's Dynamic Inventory, and how to use Ansible to extract device data from NetBox and use this to automate the generation of device configuration files using Jinja templates.

## Challenge - Improve The Jinja Templates
If you fancy a challenge why not develop these playbooks and templates further and improve them? Maybe you could add descriptions for the switch interfaces and add that logic to the JunOS templates? Maybe you could make them more efficient, or you could try using Nornir or Python instead of Ansible to pull data from NetBox and render the configs.

In [Module 8: What About Virtualization?](../8-what-about-virtualization/8-what-about-virtualization.md) you will learn how NetBox models Virtualization, including Cluster Types, Clusters, Platforms, VM's and VM Interfaces. You will also learn how to model network services and associate them with devices or VM's, along with specific IP addresses.

## Join the Discussion
If you have any questions as you go through the course then pop on over to the [NetBox Learning Channel](https://netdev-community.slack.com/archives/C0453L6565C) on the NetDev Community Slack! If you aren't already a member then you can sign up for free [here](https://netdev.chat/).

## Useful Links
- [NetBox Cloud](https://netboxlabs.com/free-netbox-cloud/) is a hosted solution offered by NetBox Labs, and you can easily get started with the Free Plan.
- [Zero To Hero Git Repo](https://github.com/netbox-community/netbox-zero-to-hero)
- [Official NetBox Documentation](https://netboxlabs.com/docs/netbox/en/stable/)
- [NetBox Docker](https://github.com/netbox-community/netbox-docker)
