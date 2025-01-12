Hello and welcome to this video for module 5 of the NetBox Zero to Hero training course. If you haven't already checked out the earlier modules yet then you can find the link to them in the notes below to get started.

For this demo I am using a NetBox Cloud instance. If you would like to follow along with the demo, then you can easily do that too. There are a couple of links down below to help you get set up with your own instance of NetBox along with a link to the notes that accompany this video module.

In this video our network engineer Eric, will populate NetBox with the Cables, Interface and Console connections data for the planned new Brisbane branch office. To do this Eric will upload the data from a CSV file. The file contains all the details of each of the cables that need to be added into the NetBox database.

OK, so we are logged into NetBox as Eric, and as you can see there is no data yet in the Connections section, under Cables. Before doing the bulk upload of cables from the CSV file lets take a look at how you would do this for a single cable.

Click on Devices and then select the WAN router and then click on Interfaces. To connect a cable between interface GigabitEthernet0/0/0 and the switch, click on the green 'connect cable' icon, and then select interface. So now you have the A side already populated, on the B-Side of the connection from the drop-down select the switch and then the interface.

Notice that Either end of a cable may terminate to multiple objects of the same type. (click another interface) For example, a network interface can be connected via a fiber optic cable to two discrete ports on a patch panel (each port attaching to an individual fiber strand in the patch cable).

OK, then below this you define the cable - starting with the status (click planned) - and note that this is the only required field here - then the type of cable - you can select from various types of copper, fibre and power cables. so select CAT6 for this one.  give it a label if you like, select a colour (red), the cable length for example 1 metre, and then select any tags you might have created.

Lastly you can select the tenancy, and then simply click create. and now you can see the connection details here and you can also get a visual trace of the cable - which is fantastic when you need to troubleshoot a physical connection, you can instantly see from here what it should be. Once again, here NetBox as the Source Of Truth is defining what the *intent* of the network is.

OK, delete this example cable, by clicking. connections - cables, and then select it and delete it.

Great - OK so obviously adding a large number of cables in this way would be tedious and error prone, so click on the 'import' icon, and here is the bulk import screen and in the CSV field options you can see the required and optional fields for a successful import.

Now this time Eric is going to upload a file rather than paste in the data. So here is the file to be used, and you will find this in the GitHub repository for this course (show file in Git Repo).

There is the header for all the fields being and the data for each cable below it. For example, the first cable has the side A device which is the router, the type is an interface, and it is interface gigabit ethernet 0. On the B side, this cable is connecting to the switch interface ge-0/0/47 - the type is CAT6, the status is planned, the tenant is the consulting department, and this is a red cable that's half a metre long.

then the other cables are the console port connections, and then further down are the connections from the access switch ports to the front ports of the patch panel.

so, back in the web interface, click on Upload File, and then choose file, select the file 'Brisbane_Cables.csv'. click open, and then click submit. And a couple of seconds later the import of all 23 cables is complete.

Then click on Interface Connections to view specifically the interfaces that are now connected. and click on Console connections to view those too.

then click on an individual device, for example the switch, and look at the interfaces tab - as well as the first interface that is connected to the router, you have the interfaces from ge-0/0/10 down that connect to the patch panel. so click one of those cables to see the details, and the A and B ends are defined, and you can see it is a purple cat 6 cable that is 50cm long.

Just go back to the device again and click on the icon to do a cable trace, and you now get the nice visual of the cable connection. and you can view and visualize all of the other cables that have just been added in the same way.

So, now that Eric has added all the required cables he can share this information from NetBox with the cabling contractor when the time comes for the installation!

So, I hope that has been a useful overview of how populate populate NetBox with your Cables, Interface and Console connections. And hopefully you now have a clear understanding of how NetBox models this kind of data.

So once again, thanks for watching!