# Module 12 - The Boss is Asking for a Report (another easy win!)

# Introduction

Hello and welcome to module 12 of the NetBox 'Zero-to-Hero' course. In [Module 11: Custom Scripts](../11-custom-scripts/11-custom-scripts.md), Eric used a custom script to update the status of the new Brisbane Site, and all the Locations, Racks, Devices, Clusters and VMs from `Planned` to `Active`. Eric used another Custom Script to create a site and all the required devices for a new branch office planned for Stockholm, Sweden.

Now that the new Brisbane site is active, the IT Manager has asked Susan to produce some reports from NetBox to make sure that everything has been deployed in line with company standards. These standards are:

1. Every device has either an IPv4 or IPv6 Primary address assigned
2. Every device meets the required naming convention standard - for example: (site_name)-RTR-1 or (site_name)-SW-2

By the end of this module you will be able to:
- Describe what NetBox reports are and what kind of things they can be used to verify
- Understand the basics of reports and also where to find documentation and examples to help you develop your own reports
- Kick start your own NetBox reports collection, with two example reports to get you up and running

## Get Hands On
If you'd like to follow along with the examples used in this course, it's super easy to do, and you have a few options:
1. Sign up for the NetBox Cloud [FREE Plan](https://netboxlabs.com/free-netbox-cloud/) - All the power and flexibility of NetBox - with the security, reliability, and management covered by NetBox Labs - for free. Select the option for no data to ensure you start with an empty NetBox database.
2. Run NetBox as a container with [NetBox Docker](https://github.com/netbox-community/netbox-docker)
3. Follow the [official documentation](https://netboxlabs.com/docs/netbox/en/stable/installation/) and do a full installation of all the NetBox components. These instructions have been tested on Ubuntu and CentOS Linux.
4. Use the public [demo instance](https://demo.netbox.dev/) of NetBox

The NetBox version used in the video for this module is `v4.1.11`, and the following course materials used in the demo are available:
- [Report Examples](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/reports)

## NetBox Reports
From the [official NetBox docs](https://netboxlabs.com/docs/netbox/en/stable/customization/reports/)
>A NetBox report is a mechanism for validating the integrity of data within NetBox. Running a report allows the user to verify that the objects defined within NetBox meet certain arbitrary conditions. For example, you can write reports to check that:
>
> - All top-of-rack switches have a console connection
> - Every router has a loopback interface with an IP address assigned
> - Each interface description conforms to a standard format
> - Every site has a minimum set of VLANs defined
> - All IP addresses have a parent prefix
>
>...and so on. Reports are completely customizable, so there's practically no limit to what you can test for.

The official documentation for NetBox reports referenced above is **the best source of information** on the subject and (as with all modules in this course), this module is meant to compliment the official docs.

## Community Reports Git Repo
As ever with NetBox, you will find that the amazing NetBox Community is a great source of help with any aspect of running NetBox, and reporting is a great example of this. In the [Community Reports Git repo](https://github.com/netbox-community/reports), you will find a collection of community submitted and maintained NetBox reports and custom scripts.

There a bunch of reports under different categories (eg. IPAM, DCIM etc) that you can either use as is or tailor to your own reporting requirements. This module will use two example reports from the community reports repo to demonstrate the functionality.

## Writing Reports
To add reports to your NetBox installation, reports should be saved as files in the `REPORTS_ROOT` path (which defaults to netbox/reports/).

If you are running NetBox Docker then you will find in your `docker-compose.yml` file the local directory `./reports` on the host you are running docker-compose on, is mounted as a volume in the NetBox container:

```
    volumes:
...
    - ./scripts:/etc/netbox/reports:z,ro
```
Simply copy your reports into `./reports` and they will appear in the NetBox container.

## Running Reports
You can run reports in one of three ways - Via the Web UI (the focus of this module) by navigating to the report, and clicking the "run report" button at top right, or via the API or the CLI. Again, refer to the official docs for more information on running reports via the API/CLI.

### Report One - Check For Missing Primary IPs
The [first report](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/reports/ip-primary-missing.py) does the following:

1. loops over all devices with a status of `active`
2. checks if each device has a primary IPv4 IP address configured
3. checks if each device has a primary IPv6 IP address configured
4. reports which devices are missing primary IPs

### Report Two - Check All Devices Meet Naming Standards
The [second report](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/reports/CheckDeviceNaming.py) does the following:

1. loops over all devices with a status of `active`
2. uses Regular Expression matching to check if the device name meets the naming standards e.g. `AUBRI01-RTR-1`
3. reports which device names do not match the naming standards

## Video - NetBox Reports
OK, so that's the overview of NetBox reports - let's see them in action!! This video will give you a quick walk through of the code in each report, and show you how to run them from the Web UI and see the results.

If you are following along, don't forget to use the [Report Examples](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/reports).


[![Netbox reports](https://img.youtube.com/vi/RH3Syxm3EKA/maxresdefault.jpg)](https://www.youtube.com/watch?v=RH3Syxm3EKA)

## Summary
In this module you have learned what NetBox reports are and what kind of things they can be used to verify. You also learned the basics of reports and where to find documentation and examples to help you develop your own reports. You also kick started your own reports collection, with two example reports to get you up and running.

## Challenge
If you fancy a challenge why not write a report of your own. We'd love to see your Reports, so feel free to share them on the [NetBox Learning Channel](https://netdev-community.slack.com/archives/C0453L6565C) on the NetDev Community Slack!

## Join the Discussion
If you have any questions as you go through the course then pop on over to the [NetBox Learning Channel](https://netdev-community.slack.com/archives/C0453L6565C) on the NetDev Community Slack! If you aren't already a member then you can sign up for free [here](https://netdev.chat/).

## Useful Links
- [NetBox Cloud](https://netboxlabs.com/free-netbox-cloud/) is a hosted solution offered by NetBox Labs, and you can easily get started with the Free Plan.
- [Community Reports Git repo](https://github.com/netbox-community/reports)
- [Zero To Hero Git Repo](https://github.com/netbox-community/netbox-zero-to-hero)
- [Official NetBox Documentation](https://netboxlabs.com/docs/netbox/en/stable/)
- [NetBox Docker](https://github.com/netbox-community/netbox-docker)
