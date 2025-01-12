# Module 2 - Setting Up The Organization
# Introduction

Hello and welcome to module 2 of the NetBox 'Zero-to-Hero' course. In [Module 1: Introduction](../1-Introduction/1-introduction.md) you learned the features of NetBox, and had a guided tour of both the Web Interface and the REST API. In this module we will expand on this by adding the Organizational data for our fictional consulting firm into NetBox.

By the end of this module you will be able to:
- Describe how NetBox models the organization using tenant groups, tenants, regions, site groups, sites, locations, racks and contacts
- Use the web interface to manually add data for an organization
- Use the web interface to bulk upload larger amounts of data for the organization

## Get Hands On
If you'd like to follow along with the examples used in this course, it's super easy to do, and you have a few options:
1. Sign up for the NetBox Cloud [FREE Plan](https://netboxlabs.com/free-netbox-cloud/) - All the power and flexibility of NetBox - with the security, reliability, and management covered by NetBox Labs - for free. Select the option for no data to ensure you start with an empty NetBox database.
2. Run NetBox as a container with [NetBox Docker](https://github.com/netbox-community/netbox-docker)
3. Follow the [official documentation](https://netboxlabs.com/docs/netbox/en/stable/installation/) and do a full installation of all the NetBox components. These instructions have been tested on Ubuntu and CentOS Linux.
4. Use the public [demo instance](https://demo.netbox.dev/) of NetBox

The NetBox version used in the video for this module is `v4.1.8`.

## The Organizational Data
Our fictional organization is structured as follows:

### NetBox System Users
Our two awesome Network Engineers, Eric and Susan will be set up with their own user accounts as they will be the two main administrators of the NetBox System.

### Tenancy
In NetBox most core objects can be associated with a tenant and this conveys ownership of the object. For example a managed service provider can associate network devices with individual customers.

They will use the Tenancy feature to define their internal business units and associate them with objects. They have a tenant group (Departments) and this contains the tenants - Sales, Finance, IT, Marketing and Consulting

### Regions
In NetBox, regions are typically used to model countries, states, and cities. Our fictional company is present in 4 regions - Africa, Asia Pacific, Europe and North America. These 'parent' regions will also contain the Country, State and City regions nested within them.

### Site Groups and Sites
Whereas regions are intended for geographic organization, site groups may be used for functional groupings. A site typically represents a building within a region and/or site group. Our fictional company has two Site Groups - Branch (consisting of ten sites) and Corporate (consisting of two sites).

### Locations
A location can be any logical subdivision within a building, such as a floor or room. All the Branch sites have a single location (Comms Room) and the Corporate sites have an extra location within them (On-Premises Data Center)

### Racks, Rack Types and Rack Roles
Racks are physical objects into which devices are installed. NetBox models each equipment rack as a discrete object within a site and location. In our example all sites have at least one rack where Network/IT equipment is installed.

Rack Types define the physical characteristics of a particular manufacturer's model of rack and you can create these to match your preferred rack types. Users can also create custom roles to which racks can be assigned. In our example we will create a couple of rack types and define rack roles for Infrastructure, Compute and Storage.

### Contacts
To complete the organizational set up we will need to add some contacts. A contact is an individual responsible for a resource within the context of its assigned role. Contacts can be members of a group, and contact roles define the relationship a contact has with an assigned object. Unique contacts are created once and can be assigned to any number of NetBox objects.

Our fictional company will use 2 contact groups - IT, which contains Susan and Eric and Facilities, which contains Alexa. The contacts will be assigned to site objects and be given one of the contact roles 'Operations' or 'Emergency'.

## Video - Setting Up The Organization
As always the best way to understand the power of NetBox is to dive right in! This video will step through creating all the objects for the fictional consulting organization. Also, as our awesome Network Engineers, Susan and Eric are using NetBox to help build out the new branch network in Brisbane, Australia, they will also add the information for this new site to kick off the project.

If you are following along you can find the [CSV data](https://github.com/netbox-community/netbox-zero-to-hero/tree/main/modules/2-setting-up-the-organization/csv_data) in course the Git repository.

With that said, let's get started!

[![Setting up the organization](https://img.youtube.com/vi/Lit6H8XF2d0/maxresdefault.jpg)](https://www.youtube.com/watch?v=Lit6H8XF2d0)

## Summary
In this module you learned how to set up your organization's data in NetBox using the Web Interface.

In [Module 3: Adding the Kit](../3-adding-the-kit/3-adding-the-kit.md),  Eric will add the devices that are going to be installed at the planned new Brisbane branch office, making use of a Postman collection to make REST API calls to NetBox.

## Join the Discussion
If you have any questions as you go through the course then pop on over to the [NetBox Learning Channel](https://netdev-community.slack.com/archives/C0453L6565C) on the NetDev Community Slack! If you aren't already a member then you can sign up for free [here](https://netdev.chat/).

## Useful Links
- [NetBox Cloud](https://netboxlabs.com/free-netbox-cloud/) is a hosted solution offered by NetBox Labs, and you can get started with the Free Plan.
- [Official NetBox Documentation](https://netboxlabs.com/docs/netbox/en/stable/)
- [NetBox Docker](https://github.com/netbox-community/netbox-docker)
