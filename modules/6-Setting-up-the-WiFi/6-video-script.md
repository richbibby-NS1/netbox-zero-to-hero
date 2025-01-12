Hello and welcome to this video for module 6 of the NetBox Zero to Hero training course. If you haven't already checked out the earlier modules yet then you can find the link to them in the notes below to get started.

For this demo I am using a NetBox Cloud instance. If you would like to follow along with the demo, then you can easily do that too. There are a couple of links down below to help you get set up with your own instance of NetBox along with a link to the notes that accompany this video module.

In this video our network engineer Susan, will populate NetBox with the Wireless LAN data for the planned new Brisbane branch office. To do this Susan is going to use some simple python scripts. Again you will find links to all of these resources in the accompanying GitHub repository.

OK, so we are logged into NetBox as Susan, and as you can see there is no data yet in the Wireless section of the home page. Susan has already followed the set up instructions for Python, and has activated a new new virtual environment. Please do check the instructions in the course notes if you need help with this initial set up of Python.

So, in VS Code let's explore the Python scripts. The first task is to create the new Wireless LAN Group, and the script for this is called 'create_wlan_group.py. So starting at top, we are importing the 'requests' module which is used to make the API calls. Next is the json module that is used to work with data in json format, and then the 'os' and 'dotenv' modules allow the script to access the local system environment.

Line 7 loads the .env file so that the API token can be read by the script. line 12 creates a variable called 'token' and sets the value from 'api_token'. which has already been added to the .env file (show file).

Lines 15-17 set some variables for the netbox system that you are connecting to and you can adjust these to match your own system. Line 20 take these variables and use their values to build the URL for the API requests. And as this script is going to create a new wireless LAN group then the API endpoint is api/wireless/wireless-lan-groups.

Lines 23 to 26 define the payload that is going to be sent in the API request - in this case there is a name, a slug and a description for new group that represents the Wireless networks in the Asia Pacific region.

Lines 28 thru 31 define the headers being used , which should be familiar to you now if you have watched earlier videos in this course. You can see on line 30 the variable for the API token is being used without the token itself needing appear in the code.

Then finally lines 34 thru 36 make the API call using the python requests module. there is a new variable created called response which has a value of the API request made using the requests module. This consists of the request type which is a POST, the URL, the headers, and the data payload.

Lines 35 and 36 take the response, which is what has been returned back from the NetBox server and prints it using some indentation to make it more human readable.

Ok, so do run this go to the terminal and from the virtual environment enter 'python3' followed by the name of the script, which is create_wlan_group.py' and hit enter. and the script has run in less than a second, and has printed the response in nicely formatted json. There is now a wireless LAN group called Asia_Pacific_WLANs, and it has an ID of 2.

To View this in the NetBox Web interface, just flip back and then click on Wireless and then Wireless LAN groups. and there is the new group.

Perfect. OK so the next task is to add the actual Wireless LAN's themselves. The script to do this is called, you guessed it - create_wlans.py. and is identical to the first script apart from 2 things - in line 20 the API end point is now /api/wireless/wireless-lans and the payload is a list containing the details for both of the new SSID's.

Now you will have noticed also that the payload contains ID values for the group, the vlan and the tenant. Now on our demo you could easily find these values from the web interface - but you could also use python scripts to get these values. So to get the ID of the wireless lan group - there is another script called get_wlan_groups - which uses a GET request that you can see on line 30 to the wireless-lans-groups API end point using the brief format. So, running that returns the group and you can see the ID of 2.

To get the ID's of the vlans and the tenants, just run the get_vlans.py script which returns the list of vlans.

(run script)

and this shows vlan ID's of 3 for the branch wifi VLAN and 4 for the guest wifi VLAN.

and then the get_tenants.py script to return the list of tenants.

(run script)

and there is the ID of 5 for the Consulting tenant.

So, now we have the values for the ID's needed to create the wireless LANs, all that remains is to run the script to add them. So, run the command 'python3 create_wlans.py' and there you go - you can see the nicely formatted json output of the response from the NetBox server.

To View the result of this in NetBox Web interface, just flip back and then click on Wireless LANs, and there they are. Both SSID' are members of the 'Asia_Pacific_WLANs' group, and click on the Branch office wifi for example and you can see all the details of the SSID, including the group, the vlan and the tenant, along with the authentication settings, which is awesome!

So, I hope that has been a useful overview of how add Wireless LAN data using some really simple Python scripts to interact with the NetBox REST API. Without diving too deep into Python, I hope you can see how easy it is to get started with some basic scripts to save you time and reduce errors when adding data into NetBox.

Interacting programmatically with the NetBox REST API using Python also opens up so many possibilities for using NetBox as your single source of truth for your network automation efforts going forward.

If you fancy a challenge why not develop these simple scripts further and improve them? Maybe you could create a Python function to add the data, or move the json data into a separate file and get your script to loop over it when it runs. It would be great to see how you develop your scripts and if you want to share this or just ask questions then pop on over to the NetBox Learning Channel on the NetDev Community Slack and join in the discussion!

So I hope you enjoyed that, and once again, thanks for watching!