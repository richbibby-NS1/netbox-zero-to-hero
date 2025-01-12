# Module 6 - Setting Up The WiFi

# Introduction

Hello and welcome to module 6 of the NetBox 'Zero-to-Hero' course. In [Module 5: Making the Connections](../5-making-the-connections/5-making-the-connections.md),  Eric added the cables and connections required to connect the devices in the new Brisbane branch office network together.  In this module, Susan will be adding the required Wireless LANs using some simple Python scripts to interact with the NetBox REST API.

By the end of this module you will be able to:
- Describe how NetBox models Wireless LANs
- Use simple python scripts to read data from and write data to the NetBox database via the REST API

## Why Use Python Scripts With NetBox?
If you have been through the previous modules of this course you will know that it's easy to add data manually via the Web Interface, but when you are dealing with a lot of data that manual process can quickly become both tedious and error prone. Using simple python scripts has the advantages of saving you time and reducing errors.

Getting started with Python is super easy and a lot of fun too! The [scripts](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/python_scripts) included in the GitHub repository for this course will get you up and running quickly and within a few minutes you can be interacting with the NetBox database programmatically via the REST API.

Also, as any Network Engineer / IT Pro looking to expand their skill set knows, Automation is becoming critical for managing modern networks, so by adding some basic Python coding skills to your armory, you will set yourself up for long term career success!

## Get Hands On
If you'd like to follow along with the examples used in this course, it's super easy to do, and you have a few options:
1. Sign up for the NetBox Cloud [FREE Plan](https://netboxlabs.com/free-netbox-cloud/) - All the power and flexibility of NetBox - with the security, reliability, and management covered by NetBox Labs - for free. Select the option for no data to ensure you start with an empty NetBox database.
2. Run NetBox as a container with [NetBox Docker](https://github.com/netbox-community/netbox-docker)
3. Follow the [official documentation](https://netboxlabs.com/docs/netbox/en/stable/installation/) and do a full installation of all the NetBox components. These instructions have been tested on Ubuntu and CentOS Linux.
4. Use the public [demo instance](https://demo.netbox.dev/) of NetBox

The software versions used in the video for this module are:
- `NetBox v4.1.8`
- `Python v3.12.5`

## Setting Up Your Python Environment
Follow these [simple steps](https://github.com/netbox-community/netbox-zero-to-hero/blob/main/python_scripts/readme.md) to set up Python on your own system, so you can follow along running the python scripts.

## Modelling Wireless LANs in NetBox
You can model Wireless LANs (and Wireless point-to-point links) in NetBox just as you can model physical cabled networks. You can create self-nesting Wireless LAN Groups, and can also (optionally) bind Wireless LANs to particular VLANs. You can also (optionally) define the authentication parameters - Type, Cipher and PSK, for each SSID (Service Set Identifier).

## The Project - New Brisbane site Wireless LANs
Susan has planned the following Wireless LANs for the new Brisbane site, and this needs to be added to NetBox.

| SSID | Description | WLAN Group | VLAN ID | Auth Type | Auth Cipher | Auth PSK |
| --- | --- | --- | :---: | --- | :---: | --- |
| B_WIFI | Branch Office Wifi | Asia_Pacific_WLANs | 30 | wpa-enterprise | aes | 5up3r5ecr3tK3y |
| G_WIFI | Guest Wifi | Asia_Pacific_WLANs | 40 | wpa-enterprise | aes | M3g45ecr3tK3y |

## Video - Adding Wireless LANs to NetBox
OK, so that's the planning and design work done - now onto the demo! This video will step you through how to populate NetBox with Wireless LAN data, and then to view that data in the Web Interface. As always the best way to understand the power of NetBox is to dive right in, so let's get started!

If you are following along you can find the [python scripts](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/python_scripts) in the course GitHub repository and you can use these as a starting point for building your collection of Python scripts for NetBox.

[![Adding wireless LANs into Netbox](https://img.youtube.com/vi/rewxPa58VCw/maxresdefault.jpg)](https://www.youtube.com/watch?v=rewxPa58VCw)

## Summary
In this module you have learned how NetBox Models Wireless LANs, and how to add Wireless LAN data using simple Python scripts.

## Challenge - Improve the Python Scripts
If you fancy a challenge why not develop these simple scripts further and improve them? Maybe you could create a Python function to add the data? Or you could try moving the json data into a separate file and get your script to loop over the json data when it runs.

It would be great to see how you develop your scripts and if you want to share this or just ask questions then pop on over to the [NetBox Learning Channel](https://netdev-community.slack.com/archives/C0453L6565C) on the NetDev Community Slack and join in the discussion!

In [Module 7: Automate All the Things!](../7-automate-all-the-things/7-automate-all-the-things.md) you will learn how to automate the generation of device configurations, by using Ansible to pull data from NetBox and render the configuration files from Jinja2 templates.

## Join the Discussion
If you have any questions as you go through the course then pop on over to the [NetBox Learning Channel](https://netdev-community.slack.com/archives/C0453L6565C) on the NetDev Community Slack! If you aren't already a member then you can sign up for free [here](https://netdev.chat/).

## Useful Links
- [NetBox Cloud](https://netboxlabs.com/free-netbox-cloud/) is a hosted solution offered by NetBox Labs, and you can easily get started with the Free Plan.
- [Official NetBox Documentation](https://netboxlabs.com/docs/netbox/en/stable/)
- [NetBox Docker](https://github.com/netbox-community/netbox-docker)
