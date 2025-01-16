Hello and welcome to this video for module 7 of the NetBox Zero to Hero training course. If you haven't already checked out the earlier modules yet then you can find the link to them in the notes below to get started.

For this demo I am using a NetBox Cloud instance. If you would like to follow along with the demo, then you can easily do that too. There are a couple of links down below to help you get set up with your own instance of NetBox along with a link to the notes that accompany this video module.

In this video our network engineer Eric, will use the Provisioning feature of NetBox to generate the device configurations for the Cisco WAN Router and the Juniper Access Switch at the new Brisbane branch office.

Before doing this Eric will add some extra data that will be used to build the device configurations, starting with the Primary IP addresses for the router and switch. So for the Router, click on devices, the name of the router,  and then select interfaces. as this device is managed over interface gigabitethernet0, select the IP address assigned to this interface and then click edit. Then scroll down to Assignment and tick the box to make this the primary IP for the device/VM. and click save.

Then do the same for the switch. so that's devices, the switch, interfaces and as this device is managed over interface ge-0/0/0, select the IP address assigned to this interface and then click edit. Then scroll down to Assignment and tick the box to make this the primary IP for the device and click save.

Great. Next add some helpful interface descriptions to the router interfaces, by selecting the device then interfaces. then click the orange edit icon next to interface GigabitEthernet0/0/0, and add a description '--> AUBRI01-SW-1 ge-0/0/0' that indicates what this interface connects to, and save it.

Do the same for GigabitEthernet0/0/1, and add a description of '--> ISP Router', then save. And lastly for GigabitEthernet0 click edit and then add '--> AUBRI01-SW-1 ge-0/0/47' the description for this interface, and click save.

Great, so that's our interface descriptions set up for the router, next add the 802.1Q settings that need to be configured for some of the switch interfaces. go to devices and select the switch, then click on interfaces and scroll down to ge-0/0/10 - select it and then also select down to ge-0/0/17, then scroll down and click edit - this allows us to do a bulk edit of all the selected interfaces - scroll down to 802.1Q switching and select the node to be access, the vlan group to be Brisbane VLANs, and the untagged vlan to be the Data vlan - with a vlan ID of 10.

Then click apply and then select interfaces 0/0/18 to 25, and click edit, and make these interfaces untagged for vlan 20 - the voice VLAN.  then click apply and then select interfaces 26 and 27 - and as these ports are going to connect to the wireless access points they need to be trunk ports, so select tagged (all) and click apply.

So now that we have finished defining the intended state of the devices, Eric can add basic jinja templates for Cisco and Juniper devices to NetBox, and also add some configuration context data that will apply to all devices and VM's in the Asia Pacific region. Eric will then render the intended device configurations in NetBox.

Once again you will find all of the files and data we are going to use in the GitHub repository that accompanies this course, and you can find a link to it in the notes below. So let's take a quick look at the templates themselves starting with the cisco-router-template.j2 file. If you need a more in depth explanation of jinja templates then you can find one on this module's page on the course web site, but for this video you can see it contains a lot of the static configuration that will be common to all our cisco devices, then on line 12 is the first variable - the device name that is coming from Netbox, and then on line 36 it is pulling in the name servers, and then further down from line 48 it is looping over the interfaces and IP addresses etc.

The juniper junos template does exactly the same thing but in the format of a Juniper device configuration, again using variables pulled from NetBox for all the dynamic data.

OK, so lets add these templates to NetBox starting with the template for the Cisco WAN Router. So, click on provisioning and then click the plus to add a template. Give it a name of Cisco Router Template, and then in the Environment parameters section, we need to add in some extra parameters in json format that ensures that all unnecessary whitespace is trimmed from the rendered configuration.

Next we paste in the contents of the template into the template code section - don't forget you can get this code from the course GitHub repository, and then click create and add another, and do exactly the same for the Juniper Switch Template and click create. Perfect, so now that we have added our templates we need to assign them to our devices.

Go to Devices and click the edit icon next to the WAN router, scroll down to Config Template and select the Cisco Router Template from the drop down menu, and then click save. Next do the same for the switch - click edit, then scroll down to Config Template and select the Juniper Switch Template from the drop down menu, and click save. Great!

Next, we'll add the config context data, and if we take a look at the file with this data in we can see that it is a list of name servers, ntp servers, the snmp read-only community, and the syslog servers in json format.

To add to NetBox, on the left hand side of the menu, select Provisioning and then Config Contexts and then click Add. The name will be 'Asia-Pacific-Config' and then paste in the data from the file. So these are the regional servers and settings that are to be used by all devices and VMs in the Asia Pacific region. For the assignment, select Asia Pacific from the drop down menu next to Regions, and then click create. great so now you can view this data in either json or yaml format.

OK so now the intended state has been defined for our router and switch we can view the generated configuration for each device. So starting with the WAN router, if I click on the device name and then click on Config Context first we can see the data for the servers and snmp configuration for all devices in the region has been pulled through. Next we click on Render Config to view the final rendered configuration where the variables have all been replaced with the actual values for this device pulled from netbox. If you wanted to you could simply click the download link to grab a local copy as a text file as well.

For the switch let's take a look at how you would access the rendered configuration using the API, which is how a provisioning system or other automation tool would access it so it could be deployed to a device automatically. so scroll down and launch the rest api documentation. once that has loaded scroll down and find the post request for rendering a device config, which is this one. click 'try it out' and then for the device ID enter 3 as this is the switch's ID, and then click execute.

now that's executed we can scroll down and view the json response....and as you can see the content contains the full configuration for the access switch including the vlans, the interfaces with IP addresses if they are layer 3, or the switch port config if they are layer 2, including the mode and the vlans for the access ports and the trunk ports.

So, I hope that has been a useful overview of how to generate device configurations natively in NetBox. I hope you can see how easy it is to do, and also how powerful this feature is for network engineers looking to automate their network with NetBox as the heart of their NetDevOps workflows!

If you fancy a challenge why not develop these playbooks and templates further and improve them? Maybe you could add descriptions for the switch interfaces and add that logic to the JunOS templates? Maybe you could make them more efficient, or you could try using Nornir or Python instead of Ansible to pull data from NetBox and render the configs.

If you have any questions as you go through the course then pop on over to the NetBox Learning Channel on the NetDev Community Slack! If you aren't already a member then you can sign up for free using the link below.

So I hope that was useful, and once again, thanks very much for watching!