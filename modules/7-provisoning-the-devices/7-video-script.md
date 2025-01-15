Hello and welcome to this video for module 7 of the NetBox Zero to Hero training course. If you haven't already checked out the earlier modules yet then you can find the link to them in the notes below to get started.

For this demo I am using a docker instance of NetBox running locally on my laptop. If you would like to follow along with the demo, then you can easily do that. There are a couple of links down below to help you spin up your own instance of NetBox, along with a link to the course that accompanies this video.

In this video our network engineer Eric, will use Ansible to extract data from NetBox and then use that data to automate the creation of device configurations for the Cisco WAN Router and the Juniper Access Switch, at the new Brisbane branch office.

To do this Eric will set up Ansible to use NetBox as the source for it's Dynamic Inventory. He'll run Ansible playbooks to make calls to NetBox REST API end points to extract the required data for the device configurations, and then use Ansible again to automate the generation of device configuration files using Jinja templates, passing in the data extracted from NetBox as variables.

Once again you will find links to all of the resource used in the GitHub repository that accompanies this course, and you can find a link to it in the notes below.

Eric has already followed the set up instructions for Ansible, has activated a new new virtual environment, and has set his environment variables up for the API URL and the API TOKEN. please do check the instructions in the course notes if you need help with this initial set up of Ansible.

OK, so we are logged into NetBox as Eric, and there are a few extra things to do in NetBox before we generate the device configurations. We'll add some extra information that we can use to build the device configurations.

Firstly, add the config context that will apply to all the devices in the Asia Pacific region - to do this on the left hand side of the menu, select other and then scroll right down to 'Config Contexts' and then click Add.

the name will be 'Asia-Pacific-Config' and then add the data in json format. this contains a list of name servers, ntp servers, the snmp read-only community, and he syslog servers. So these are the regional servers and settings that are to be used by all devices and VMs in the Asia Pacific region. so for the assignment, select Asia Pacific from the drop down menu next to Regions, and then click create. great so now you can view this data in either json or yaml format.

Next set the Primary IP addresses for the router and switch so that when the Ansible Inventory is generated from NetBox, Ansible knows which IP address to connect to when it needs to interact with the devices. So for the Router, click on devices and then select interfaces. as this device is managed over interface gigabitethernet0, select the IP address assigned to this interface and then click edit. Then scroll down to Interface Assignment and tick the box to make this the primary IP for the device/VM. and click save.

then do the same for the switch. so that's devices, the switch, interfaces and as this device is managed over interface ge-0/0/0, select the IP address assigned to this interface and then click edit. Then scroll down to Interface Assignment and tick the box to make primary and click save.

Great. Next add some helpful interface descriptions to the router interfaces, by selecting the device then interfaces. then click the orange edit icon next to interface GigabitEthernet0/0/0, and add a description '--> AUBRI01-SW-1 ge-0/0/0' that indicates what this interface connects to, and save it.

Do the same for GigabitEthernet0/0/1, and add a description of '--> ISP Router', then save. And lastly for GigabitEthernet0 click edit and then add '--> AUBRI01-SW-1 ge-0/0/47' the description for this interface, and click save.

Great, so that's our interface descriptions set up for the router, next add the 802.1Q settings that need to be configured for some of the switch interfaces. go to devices and select the switch, then click on interfaces and scroll down to ge-0/0/10 - select it and then also select down to ge-0/0/17, then scroll down and click edit - this allows us to do a bulk edit of all the selected interfaces - scroll down to 802.1Q switching and select the node to be access, the vlan group to be Brisbane VLANs, and the untagged vlan to be the Data vlan - with a vlan ID of 10.

then click apply and then select interfaces 0/0/18 to 25, and click edit, and make these interfaces untagged for vlan 20 - the voice VLAN.  then click apply and then select interfaces 26 and 27 - and as these ports are going to connect to the wireless access points they need to be trunk ports, so select tagged (all) and click apply.

OK so now we have defined some additional device data, we can can use Ansible to generate configurations for the router and the switch. Firstly check that the NetBox dynamic inventory for ansible is working - in VS code you can see the settings for this in the ansible.cfg file. Line 2 specifies that Ansible should use the the file called netbox_inv.yml so if we open that file you can see on line 1 it is using the netbox inventory plugin for Ansible and that on lines 4 and 5 we have asked for the devices to be grouped firstly by device role and then by sites. You can tailor this to however you need your ansible hosts to be grouped - just check the link to the plugin documentation in the course notes for this module.

So this inventory is dynamic meaning that if you add or remove devices from netbox they will be added or removed from the Ansible inventory. so to check what the Ansible inventory looks like, just run the command 'ansible-inventory --list to display a list of all the hosts, followed by '-i netbox_inv.yml' and hit enter.

If you scroll back up to the top it starts with the host variables for each host starting with the first wireless access point in Brisbane. and if you scroll down further you see the WAN router, and as we selected an IP address to be the primary IP for this device - this now appears as the Ansible host - so ansible knows which IP to connect to.  Also further down you have all the other host variables for this device which are all being pulled directly from netbox and can be used in Ansible playbooks.  and then it's the same for all the other devices and then you get to the groups which are based on the device role and the sites.

so for example if you ran a playbook that needed to perform some automation task against all the access switches, you would set the hosts in the playbook to be device_roles_access_switch or if you wanted to run a playbook against all the devices at the Brisbane site then you would target the sites_aubri01 group. There is much more to this that is beyond the scope of this course - but at least this shows you how easy it is to connect Ansible to NetBox and use it for the dynamic inventory for Ansible.

Ok so that's the inventory taken care of - lets have a look at the playbooks and templates. First of all there is the main playbook called generate_config.yml which has one task - that is run on the localhost - the ansible controller - as as we are not connecting to any actual devices with these playbooks, and targets 2 groups - device_roles_access_switch, device_roles_wan_router - which in this case will just be the router and switch for brisbane. and this playbook is calling the 'generate_config' role.

so if we check the role and the main.yml file in the vars directory we now see a new variable on line 4 called configs_directory: which is used set where the finished configuration files will be saved to. So moving onto the playbook itself there are 7 main tasks plus a bonus task that we'll get to in a second. so let's break them down:

task 1 - use the Ansible URI module on line 4 and makes an API call to the dcim/devices endpoint and filters on the device name that matches the Ansible inventory hostname. it then registers the output as a variable called 'device' on line 10. the output is the json response back from the netbox server.

task 2 does the same thing but this time is requesting the interfaces for the device on line 14. and again registers the json output in a variable called interfaces. Task 3 does exactly the same but for IP addresses and then task 4 gets the site information based on the results of play 1 as line 34 is making a call to sites API endpoint and filtering it based on the site name found in the json data for the device.

Task 5 gets a list of all the VLANs for the site and registers them in a variable called 'vlans's.

Lines 52 to 61 can be uncommented if you need to debug - for example if you wanted to view the json response being returned by one of the API calls while the playbook is running, you can just uncomment the lines and in this case it would output to the display the content of the interfaces variable - this is really useful when you are developing your playbooks and need to check the response being received back from NetBox.

So up to this point the playbook has only been collecting data from NetBox. Play 6 uses the file module to create the directory for the configuration files to be stored in, and then a subdirectory for each device based on the inventory hostname.  and then play 7 is where the real magic happens!

it uses the Ansible template module and specify's the source of the template which is in directory called 'templates' and uses the 'platform's variable which is a list and takes the first element of the list. You can see this better if you run the inventory list again ....

If you scroll up you see 'platforms' under the host variables which is a list that contains a single item - in this case the value 'juniper-junos' so the play is looking for a template file called juniper-junos.j2 in this case and for the router (if you scroll up a bit) it the value for platforms is 'cisco-ios'

then on line 71 the destination for the finished configuration file is the configs directory and withing that is a directory based on the hostname, and then the filename consists of the inventory hostname with .conf as the extension.

the last play is a bonus for all JunOS fans out there and this play takes the same device data that we already have but uses a different template file that renders the final configuration in the Junos 'set' command format. for this one note that there is an conditional statement on line 77 - which means that this play is only run when the platform is 'juniper-junos'.

So let's take a quick look at the templates themselves starting with the cisco-ios.j2 file. If you need a more in depth explanation of jinja templates then you can find one on this modules page on the course web site, but for this video you can see it contains a lot of the static configuration that will be common to all our cisco devices, then on line 13 is the first variable - the device name that is coming from Netbox, and then on line 37 it is pulling in the name servers, and then further down from line 49 it is looping over the interfaces and IP addresses etc.

The juniper junos template does exactly the same thing but in the format of a Juniper device configuration, again using variables pulled from NetBox for all the dynamic data. And the last template is for juniper again, but this time in the set command format.

Ok so with that said, it's time to run the playbook! so the command to do this is 'ansible-playbook generate_config.yml'

Ok so that ran successfully and from the output we can that for the bonus Junos play the router was skipped as we expected. and we can see the yellow output for 6, 7 and the junos bonus which means there were changes made - which are the creation of the folders and the configuration files themselves. So let's check them out.

on the left there is a new directory called configs, and inside it there is a directory for each device that has a config file in it. so if you open the router config file there is the final rendered config file where the variables have all been replaced with the actual values for this device pulled from netbox.

If you check the switch config file you have the full device config, including the vlans, the interfaces with IP addresses if the are layer 3, or the switch port config if they are layer 2 including the mode and the vlans for the access ports and the trunk ports. lastly if you check the file ending in .set for the switch you have the same configuration in the junos set command format!

So, I hope that has been a useful overview of how to set up Ansible to use NetBox as the source of it's Dynamic Inventory, and how to automate the generation of your device configurations using Ansible and Jinja templates combined with the data pulled out of NetBox!  I hope you can see how easy it is to do this with NetBox and can how powerful this combination is for network engineers looking to automate their network with NetBox as the heart of your NetDevOps workflows!

If you fancy a challenge why not develop these playbooks and templates further and improve them? Maybe you could add descriptions for the switch interfaces and add that logic to the JunOS templates? Maybe you could make them more efficient, or you could try using Nornir or Python instead of Ansible to pull data from NetBox and render the configs.

If you have any questions as you go through the course then pop on over to the NetBox Learning Channel on the NetDev Community Slack! If you aren't already a member then you can sign up for free using the link below.

So I hope that was useful, and once again, thanks very much for watching!