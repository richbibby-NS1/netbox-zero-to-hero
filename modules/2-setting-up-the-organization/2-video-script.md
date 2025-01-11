Hello and welcome to this video for module 2 of the NetBox Zero to Hero training course. If you haven't already checked out the first module yet then you can find the link in the notes below to get started. For this demo I am using a NetBox Cloud instance, and if if you would like to follow along with the demo, then you can easily do that too. There are a couple of links down below to help you get set up with your own instance of NetBox along with a link to the notes that accompany this video module.

In this video we will be laying the foundations of the NetBox data for our fictional organization. This organizational data is critical and according to NetBox best practice should be added at the start - as everything else is built on top of it.

### NetBox System Users
OK, so I am logged into NetBox as the admin user and as you can see I have a completely empty database. The very first thing to do is set up the Network Engineers Eric and Susan as users on the system, as they are going to be the 2 main users for now. One thing to note is that if you are following along using the netbox cloud free plan then the method for adding users is slightly different as you invite them to join your organization as administrators via the netbox cloud admin console. For this video though I will show you how to add them through the NetBox Web UI.

By setting up separate user accounts for them it is easy to track what changes each user is making to the database. To do this, go to Admin, Users and click add, then enter the username of Susan and a password - then select staff and superuser status which means Susan can access the both the NetBox Admin functions and has all permissions to the NetBox database. Scroll further down and you can see that explicit permissions can be set and this is the way you can be more granular with user permissions - for example you could restrict a user or a group to only have read-only access to Device data - this could be helpful in a scenario where you have junior IT support staff who might need to check what devices are at a certain location, but you don't want them to make any changes.

For now though, just click 'Create and add another' down at the bottom, and add Eric now with the same permissions as Susan. OK now that's done, so log out as the admin account and now log back in as Susan. Great, so we can stay logged in as Susan to do the rest of the work for this module.

### Tenancy

First of all Susan needs to set up the tenants, and she is using the Tenancy feature to define the internal business units and associate them with objects.

The individual tenants will be members of a tenant group - so click organization and then click the plus sign next to 'tenant groups'. Then give it the name of 'Departments', - note how the slug is automatically generated and then simply click 'create' and that is the tenant group set up.

Ok, so next add the tenants by clicking 'Add Tenant' in the top right corner. so the first one is Sales that in - and note that the group has been pre populated as we are already working within the tenant group.

then, click 'create and add another' and then do the same for Finance, IT, Marketing and Consulting. Great, then click on Tenants to get a nice table view of them all now.

So, that's the tenants set up based on the departments and Susan can now associate these tenants with other objects as she adds them going through.

### Regions

Next, set up the regions and as you know from the course notes the company is present in 4 parent regions - Africa, Asia Pacific, Europe and North America, with sub-regions nested within them.

From Regions Susan could click add here (click on add to show the interface) just like we did for tenants, but as she needs to add quite a few regions it makes sense to bulk import them to save time.

To do this, simply click on regions and then click on the import icon in the top right - and here there is an option to either paste in the data in either CSV, JSON or YMAL format, or upload the data as a file. Note the Field options section at the bottom indicates which fields are required as a minimum for the data to be accepted, and in this case it's name and slug.

Now within their Regions, our example company has nested regions for example the City of Los Angeles, is a sub-region of the State of California, which is in the United States - and the USA's parent region is North America.

I'm sure you get the picture - so we'll use the 'parent' field here too (add 'parent' header) when adding the sub-regions.

So to start, add the top level regions, by pasting in the names and slugs:

Africa,africa,
Asia Pacific,asia-pacific,
Europe,europe,
North America,north-america,

and as these don't have parents, just add a comma, but leave the value empty for the header column.  Then add the other regions that are nested below them, by pasting in those too, and where they have a parent region, include that also:

So, paste in the countries:

Australia,australia,Asia Pacific
South Africa,south-africa,Africa
New Zealand,new-zealand,Asia Pacific
Malaysia,malaysia,Asia Pacific
United Kingdom,united-kingdom,Europe
Canada,canada,North America
United States,united-states,North America

then add the US states

California,california,United States
Colorado,colorado,United States
Illinois,illinois,United States

then finally down to the City level:

Johannesburg,johannesburg,South Africa
Brisbane,brisbane,Australia
Melbourne,melbourne,Australia
Sydney,sydney,Australia
Auckland,auckland,New Zealand
Kuala Lumpur,kuala-lumpur,Asia Pacific
London,london,United Kingdom
Montreal,montreal,Canada
Denver,denver,Colorado
Los Angeles,los-angeles,California
Chicago,chicago,Illinois

OK note here that Brisbane is included as a region - and this will be the geographical location of the the new site that Eric and Susan are going to be deploying.

Ok great, so click on 'Submit' and there are all of the regions created using the bulk upload method. You can see how the regions are nested - for example Johannesburg, sits under South Africa, which comes under the Africa parent region.

### Site Groups and Sites

Alright, so next up Susan needs to set up the Site Groups and Sites. Site groups are used for functional groupings, and a site typically represents a building within a region and/or site group.

So, we have 2 Site Groups based on the function of the sites. the first is Branch so add that manually by clicking on organization and then the plus sign next to Site Groups. So the name is entered as 'Branch' and add a description of 'Branch Sites'

click create + add another, and this one is 'Corporate' for Corporate Sites.

OK, so now the Site Groups are set up (click on org-site groups) go ahead and manually add the site for the new Brisbane branch office. So this will be a member of the Branch group (click branch) and then click 'add site'.

So the name is 'Brisbane', and leave the default value for the slug. As this site is not active yet, set the status to 'planned'. From the drop down, select 'asia pacific - australia - brisbane'.

This is a branch site so there's no need for a provider or facility. similarly there is no ASN for this site, so skip that. Set the time zone to be 'australia/brisbane' and add a description of 'Brisbane Branch Site'

There are no tags set up yet so skip that, but do select the 'Departments' tenant group and then the 'consulting' department as the tenant.

Next, add in the address and co-ordinates:

30 Mills Street BRISBANE, Queensland(QLD), 4000
-27.611508
152.903083

And then just go ahead and click create, down at the bottom here. And that's it! The new Brisbane site is set up.

OK so as you know our fictional company has a number of other sites around the world so make use of the bulk upload feature again to save some time. This time go to Sites and click the upload link and here is the form to submit the data again. So we'll use CSV format again and for these sites add the name, slug, status, region, group and tenant fields

**netbox_sites.csv**

So just paste them in and this time set the status of them to be active, click submit and that's the import completed. Then click on sites and here is the full list of sites including the planned new site in Brisbane.

### Locations

So now the sites are set up, the next step is to add the locations - to re-cap, a location can be any logical subdivision within a building, such as a floor or a room. All Branch sites have a single location for IT equipment (the Comms Room) and the Corporate sites have the comms room plus an additional location within them (the on premises data center).

So once again Susan can manually add the the location in the new Brisbane site, and use the bulk upload for the other sites from CSV data. So from locations click 'add' and then select the site as Brisbane, the name is Comms Room, status is 'planned', pop in a description of 'Main IT Suite'. then lastly the tenant group is Departments and the tenant at this location is the Consulting department. OK, so go ahead and click create on that, and there is the new location all set up.

OK, so now click Locations and then the upload button for the bulk import of the rest of the locations. we'll set the headers to site, name, slug, status, tenant, and description, and paste in the rest of data. Notice how for the London and Chicago sites there are 2 locations - the 2nd one being the on-premises data center.

**netbox_locatons.csv**

Then click submit. And that's now imported 11 locations successfully, so click on Locations to view all the locations.

### Racks, Rack Types and Rack Roles
Racks are physical objects into which devices are installed. NetBox models each equipment rack as a discrete object within a site and location. In our example all sites have at least 1 rack where Network or IT equipment is installed.

You can create re-useable rack types, and our company has standardized on a half height rack for smaller deployments and a full height rack for larger sites. You can also define rack roles to which racks can be assigned, and our company has defined their rack roles as Infrastructure, Compute and Storage.

So first of all to create the rack types, if I click on Racks and Rack Types we can see the warning that "Before you can add a rack type you must first create a manufacturer." so let's do that click add. so our first rack manufacturer is HPE so I'll add that, and then add another for Vertiv, and click Create.

Great so now we can import the rack types a csv data. so under rack types I'll click on import and then paste in the data from the csv file. (you will find all the data we are importing in csv files in the Github repository that accompanies this course).

So from this data you can see we have 2 rack types, one from Vertiv that is 24 RU in height for smaller deployments such as Brisbane, and a full height rack from HPE use for for larger deployments. I'll click submit and then you can see our two new rack types are available for us to use.

Next, to create the Rack Roles, expand Racks and click the plus sign next to Rack Roles and the first one to add is 'Infrastructure', leave the color as grey and add a description of 'Mixed IT Infrastructure', the click create+add another. Next to add is the 'Compute' role select Amber, this doesn't need a description as the clue is in the name. Then add the last role type of 'Storage', go with Brown and click create. (then click rack roles)

OK so the new Comms Room location at the Brisbane site has a single rack that is used for mixed IT infrastructure - so click the 'infrastucture' rack role and then click 'add rack. The site is Brisbane, the location is the 'comms room', give it the name of 'AUBRI01-RK-01', status is 'planned' again, the role is already set to 'infrastructure'.

Now at this is branch office site the requirement is only for a half height rack so we will will select that for the rack type, the tenant group is 'Departments', and the tenant is 'Consulting', then click create. So there is the new rack for Brisbane all set up.

**NEEDS 4.1.8**
Now to add more racks there is the option to clone an existing one in the top right, or you can do the bulk import again. So paste in the data for the existing racks with the headers of site,location,name,tenant,status,role,rack_type??:

**netbox_racks.csv**

Note here that the London and Chicago sites each have 2 locations - the Comms Room and the on premises data center - and the data center racks are full height 42 rack units and there is a mixture of roles - infrastructure, Compute and storage.

So once again, click on submit and NetBox successfully imported all of the racks! (click racks)

**NEEDS 4.1.8**

### Contacts
To complete the organizational set up Susan is going to add some contacts. A contact is an individual responsible for a resource within the context of its assigned role. Contacts can be members of a group, and contact roles define the relationship that a contact has with an assigned object. Unique contacts are created once and can be assigned to any number of NetBox objects.

So to start off, our company has 2 contact groups - IT and facilities management. So from Organization and contact groups, click add and then the first one is IT, with a description of IT Staff, the 2nd one is Facilities Management, with a description of 'Facilities Management Staff'

Next add the contact roles, again clicking add. the first role to add is Operations, and then the next role will be Emergency.

Alright, so now add the individual contacts - starting with Susan - now she is in the IT contact group, and she is an Awesome Network Engineer. and then add her phone 555-123-4567 and email susan@example.com.

Then do the same for Eric who is also in the IT group, and works with Susan as an Awesome Network Engineer. he can be reached on the same number 555-123-4567, and has his email address eric@example.com

OK the last contact to add is Alexa who works in the Facilities Management team and is the Facilities Manager there. so complete her contact details as usual(555-765-4321 and alexa@example.com). OK so then click contacts to view the list.

Now contacts are assigned to objects and most core objects in NetBox can have contacts assigned to them. and the way that we will assign them is at the Site Group level. So go to Site Groups, and then click the Branch site group, then click Contacts and then click on add a contact.

From the group, select the IT Group, then Susan as the contact, select operations for the contact role, and then set Susan to be the primary contact. then add another and this time select Eric and make him the secondary contact. Lastly add another this time select facilities management, and then Alexa as the contact, giving her the role of Emergency contact. and then click on create.

SO we can now see the contacts that have been assigned to our Branch Site Group. The point here is that there is a lot of flexibility when it comes to assigning contacts, but the design is that you create the contact only once and then assigned it to as many objects as required, in a manner that suits your own organization.

So, I hope that has been a useful overview of how to model an organization within NetBox, and hopefully you had fun following along on your own NetBox instance! Thanks for watching.