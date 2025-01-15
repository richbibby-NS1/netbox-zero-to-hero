# Module 11 - Custom Scripts

# Introduction

Hello and welcome to module 11 of the NetBox 'Zero-to-Hero' course. In [Module 10: Providers and Circuits](../10-providers-and-circuits/10-providers-and-circuits.md), Network Engineer Susan added the Circuit to connect the new Brisbane branch office to the Internet.

Now it's time for the Brisbane office to 'go live', so Eric is going to use a custom script to update the status of the Site, and all Locations, Racks, Devices, Clusters and VMs at the site from `Planned` to `Active`. Using a script to do this will be much quicker and more convenient than having to go into each section of the NetBox UI to update the status of all the objects. He can simply run the script to update everything in a couple of seconds!

Eric's boss has also just informed him that a new branch office is planned for Stockholm, Sweden and that he should plan for this also. As the company has standardized on the same network equipment for all branch office locations, Eric is going to use another Custom Script to create the planned site, as well as the network devices that he is planning to deploy there.

By the end of this module you will be able to:
- Describe what Custom Scripts are in NetBox and what kind of tasks they can be used to accomplish
- Understand the basics of writing Custom Scripts and also where to find documentation to help you develop your own scripts
- Kick start your own Custom Scripts collection, with two example scripts to get you up and running

## Get Hands On
If you'd like to follow along with the examples used in this course, it's super easy to do, and you have a few options:
1. Sign up for the NetBox Cloud [FREE Plan](https://netboxlabs.com/free-netbox-cloud/) - All the power and flexibility of NetBox - with the security, reliability, and management covered by NetBox Labs - for free. Select the option for no data to ensure you start with an empty NetBox database.
2. Run NetBox as a container with [NetBox Docker](https://github.com/netbox-community/netbox-docker)
3. Follow the [official documentation](https://netboxlabs.com/docs/netbox/en/stable/installation/) and do a full installation of all the NetBox components. These instructions have been tested on Ubuntu and CentOS Linux.
4. Use the public [demo instance](https://demo.netbox.dev/) of NetBox

The NetBox version used in the video for this module is `v4.1.11`, and the following course materials used in the demo are available:
- [Custom Script Examples](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/custom_scripts)

## Custom Scripts in NetBox
From the [official NetBox docs](https://netboxlabs.com/docs/netbox/en/stable/customization/custom-scripts/)
>Custom scripting was introduced to provide a way for users to execute custom logic from within the NetBox UI. Custom scripts enable the user to directly and conveniently manipulate NetBox data in a prescribed fashion. They can be used to accomplish myriad tasks, such as:
>
>- Automatically populate new devices and cables in preparation for a new site deployment
>- Create a range of new reserved prefixes or IP addresses
>- Fetch data from an external source and import it to NetBox
>
>Custom scripts are Python code and exist outside of the official NetBox code base, so they can be updated and changed without interfering with the core NetBox installation. And because they're completely custom, there is no inherent limitation on what a script can accomplish.
>
>A custom script can prompt the user for input via a form (or API data), and is built to do much more than just reporting. Custom scripts are generally used to automate tasks, such as the population of new objects in NetBox, or exchanging data with external systems.
>
>The complete Python environment is available to a custom script, including all of NetBox's internal mechanisms: There are no artificial restrictions on what a script can do. As such, custom scripting is considered an advanced feature and requires sufficient familiarity with Python and NetBox's data model.

The official documentation for Custom Scripts referenced above is **the best source of information** on the subject and (as with all modules in this course), this module is meant to compliment the official docs. A quick internet search will also bring up plenty of example scripts that you can use or get inspiration from for your own scripts.

## Adding Custom Scripts to NetBox
To add custom scripts to your NetBox installation, scripts should be saved to `/opt/netbox/netbox/scripts`.

If you are running NetBox Docker then you will find in your `docker-compose.yml` file the local directory `./scripts` on the host you are running docker-compose on, is mounted as a volume in the NetBox container:

```
    volumes:
...
    - ./scripts:/etc/netbox/scripts:z,ro
```
Simply copy your scripts into `./scripts` and they will appear in the NetBox container. See this [discussion](https://github.com/netbox-community/netbox/discussions/6085) for more details.

## Running Custom Scripts
You can run scripts in one of three ways - Via the Web UI (the focus of this module) by navigating to the script, completing any required form data, and clicking the "Run Script" button, or via the API or the CLI. Again, refer to the official docs for more information on script execution via the API/CLI scripts.

## Custom Script One - Site Status Bulk Updater
The [first script](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/custom_scripts/SiteStatusBulkUpdater.py) takes the following as user input:

- Site Name
- Site Status
- Location Status
- Rack Status
- Device Status
- Cluster Status
- VM Status

For each input the user selects the new status - so for example if you are moving a site from `planned` to `active` then you would simply select `active` for the status of each one and then run the script. All the standard choices are available so you could also use this script to update the status to anything you like - for example to decommission a site.

## Custom Script Two - New Branch
This [script](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/custom_scripts/NewBranchScript.py) is an adapted version of the example script from the official Docs and takes the following as user input:

- Site Name
- Switch Count + Model
- Router Count + Model
- AP Count + Model
- Server Count + Model

When the script is run it will create a new Site, along with the the required routers, switches, Wireless APs and servers. This one is another great time saver in that you can set up a new site and all the required devices in a couple of seconds, all within the same screen of the UI!

## Video - NetBox Custom Scripts
OK, so that's the overview of the scripts that will be used - let's see them in action!! This video will give you a quick walk through of the code in each script, and show you how to run them from the Web UI and see the results.

If you are following along, don't forget to use the [Custom Script Examples](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/custom_scripts)

[![Netbox custom scripts](https://img.youtube.com/vi/mBZ8HGVuZyE/maxresdefault.jpg)](https://www.youtube.com/watch?v=mBZ8HGVuZyE)

## Summary
In this module you have learned what Custom Scripts are in NetBox and what kind of tasks they can be used to accomplish. You also learned the basics of writing Custom Scripts and where to find documentation to help you develop your own scripts. You also got a head start on writing your own Custom Scripts collection, with two example scripts to get you up and running.

In [Module 12: The Boss is Asking for a Report (another easy win!)](../12-the-boss-is-asking-for-a-report/12-the-boss-is-asking-for-a-report.md) you will learn what NetBox reports are and what kind of things they can be used to verify. You will also learn the basics of reports and where to find documentation and examples to help you develop your own reports. You also Kick start your own reports collection, with two example reports to get you up and running.

## Challenge
If you fancy a challenge why not see if you can develop one of the scripts further - for example you could also create the rack in the New Branch script, and then assign all newly created devices to the next available rack unit positions. Or you could also add in patch panels, console servers and PDU's - to make adding a new branch even easier! We'd love to see your scripts, so feel free to share them on the [NetBox Learning Channel](https://netdev-community.slack.com/archives/C0453L6565C) on the NetDev Community Slack!

## Join the Discussion
If you have any questions as you go through the course then pop on over to the [NetBox Learning Channel](https://netdev-community.slack.com/archives/C0453L6565C) on the NetDev Community Slack! If you aren't already a member then you can sign up for free [here](https://netdev.chat/).

## Useful Links
- [NetBox Cloud](https://netboxlabs.com/free-netbox-cloud/) is a hosted solution offered by NetBox Labs, and you can easily get started with the Free Plan.
- [Zero To Hero Git Repo](https://github.com/netbox-community/netbox-zero-to-hero)
- [Official NetBox Documentation](https://netboxlabs.com/docs/netbox/en/stable/)
- [NetBox Docker](https://github.com/netbox-community/netbox-docker)
