Hello and welcome to this video for module 4 of the NetBox Zero to Hero training course. If you haven't already checked out the earlier modules yet then you can find the link to them in the notes below to get started.

For this demo I am using a NetBox Cloud instance. If you would like to follow along with the demo, then you can easily do that too. There are a couple of links down below to help you get set up with your own instance of NetBox along with a link to the notes that accompany this video module.

In this video our network engineer Susan, will populate NetBox with the IP addressing and VLAN data for the planned new Brisbane branch office. To do this Susan is using Ansible, and in particular the Ansible Galaxy collection for NetBox. Again you will find links to all of these resources in the accompanying Github repository.

OK, so we are logged into NetBox as Susan, and as you can see there is no data yet in the IPAM section in the centre of the home page. Susan has already followed the set up instructions for Ansible, has activated a new new virtual environment, and has set her environment variables up for the API URL and the API TOKEN. please do check the instructions in the course notes if you need help with this initial set up of Ansible.

So, if you not familiar with Ansible - it has a feature called 'roles' which allows you to structure your playbooks in a very nice way - for example if I expand the roles directory, you'll see it contains subdirectories for each of the roles we have playbooks for - for example if you expand the assign_ip_addresses role, it has 2 further directories - tasks, and vars. within tasks there is a file called main.yaml -

this is what's known as a playbook in ansible terms. it has a name, and then then on line 3 you san see it is using the netbox_ip_address module. This module then takes data in the form of a dictionary in lines 4 through to 7. Lines 4 & 5 are simply the netbox API and api token variables that Ansible will lookup from the environment on the local system. Note that anything within quotes and double curly braces is a variable that ansible will look up at time the playbook is executed.

Line 6 is another dictionary of key value pairs, which I will come to in a second. line 7 is the state which is set to 'present' meaning that the ansible playbook will ensure that this data exists in NetBox when it runs - another state you could use here would be 'absent' meaning that the data would be removed from Netbox.

Lines 8 to 11 are ansible's loop mechanism - which tell's it to loop over all the items in the data dictionary when it runs. Notice that line 8 tells the the playbook to loop over the variable called ip_addresses - and this is found in the file called main.yml in the vars directory. So if you take a look at that you can see that this again is a data dictionary in yaml format, which has key/value pairs for each of the IP addresses we are going to add.

Breaking down the first one - the assigned object is the device and interface that we are assigning the IP address to. Then the prefix that we are requesting an IP address from, along with the status - in this case as the new site is not live yet, the status is reserved, and lastly the tenant - which is the Consulting department.

So, the playbook will loop over this list of ip_address assignments until it has processed them all.  Now there are other data parameters that you might want to include when adding an IP address and you can refer to the documentation for the ansible collection for more information and examples.

(open https://docs.ansible.com/ansible/latest/collections/netbox/netbox/)

you can scroll down and find the module you are looking for, in this case its the netbox_ip_address module and here is the full list of parameters and some example playbook code too.

Now each of the roles we have defined is structured in exactly the same way as this one - so feel free to explore the rest of them yourself in the roles directory. (click on and expand create_vlan_groups)

OK, so to run of all these playbooks to populate the IPAM data, you need to run the main playbook called 'populate_netbox_ipam.yml' so lets just run through what that does (open file)

So Ansible is all about playbooks and this playbook has a number of plays in it - each one calling one of the roles we have just looked at - and the playbook will execute each play in order.  Breaking this down further we can see that play 1 is calling the 'create_rirs' role - which is going to add the RFC 1918 RIR into netbox. The other parameters in each play are:

- connection: local
- hosts: localhost
- gather_facts: False

This means that the playbook is being executed on the local machine and not connecting to any remote hosts, and as such we do not need to gather any facts about the local machine when the playbook executes.

Ok, and then the rest of the playbook has plays 2- through 6, which call the other roles that we need to run to populate the IPAM data in NetBox.

OK so this is all great, but you must be thinking 'can you just run the playbook now and get to the good part!' OK, to do that, you switch to terminal and enter the command

'ansible-playbook populate_netbox_ipam.yml' and hit enter to kick off the playbook run....

and you can see how the playbook runs through each of the plays, that call the roles to be executed. And that completed OK, so running though the output from the top you can see that all the plays have completed and for each play there is a list of each item that was looped over from the data in the vars/main.yaml file for each role.

For example there was only a single RIR created - RFC 1918, but there were 3 aggregates created. moving down there are the Prefix and VLAN roles, the Supernet prefix, the vlan group, and the vlans. When you see output in yellow like this it means that Ansible changed something when it was required. In this case the requirement was for all of the items to be present in the NetBox database, and as they were not already present then Ansible created them. and at the bottom in the play recap it states that there were 6 changes - 1 for each of the plays.

One term you will hear a lot with Ansible is 'idempotency' which essentially means that the result of a successfully performed request is independent of the number of times it is executed. So in our case this means that we can run the playbook again and it will not make any more changes to NetBox as the data we wanted to add is now present.

And we can prove this by running the playbook again, and as it completes notice how the output is now green meaning that Ansible knows that the data is already present in NetBox so it does not make any more changes, and in the play recap you can see there were no changes made.

OK, so lets's flip on over back to the NetBox web interface and hit refresh. and we can see there is now some IPAM data in there! dig in a little to explore what's there starting with RIRs - there is RFC 1918, and it has 3 aggregates. 192.168.0.0/16 now has a prefix and some utilization. so click on that and then prefixes

there is the Brisbane site supernet with a reserved status - and note how the remaining IP space has been carved up automatically ready for future use.

OK so, next click Prefix and VLAN roles - so there are the roles we created. Click on VLAN Groups, and there is the Brisbane_VLANS group and it has a scope of the Brisbane site and it has the 6 VLANS that were also created by Ansible.

So far, so good! and now that we have the main IPAM data populated including the Supernet - we need to add the prefixes - and there is a separate playbook to do that called 'create_prefixes' and if we look at this one it has a single play, that calls the 'create_prefixes' role.

So if you go to the role and open the main.yaml file in the tasks directory you can see that this playbook is using the netbox prefixes ansible module, and looping over a list of key/value pairs for each prefix we need to create  - but note the extra parameter here - first_available - with  value of yes. this tells netbox to assign the first available prefix. and if you look at the data this playbook is looping over you can see that the data dictionaries consist of the parent prefix (which is the supernet for Brisbane), the prefix length, the site the tenant and the VLAN.

So run this playbook to create the prefixes, with the command 'ansible-playbook create_prefixes.yml'

and this completes OK - remember with this one as it is requesting the next available prefix each time - every time you you run it, it will try to allocate a new prefix, so you would not run this one multiple times.

OK, great so the next playbook in the collection is going to create some virtual interfaces for each of the vlans on the access switch - so we can then assign them the IP addresses.  so to run this one it is ansible-playbook create_vlan_interfaces.yml. and again this one is looping over a list of dictionaries defining the interfaces. and that has completed OK.

And the last playbook to run is the one to assign individual IP addresses from the prefixes we already created, to the interfaces on our devices. Again this one will take the first available IP address when it is executed.

so that is 'ansible-playbook assign_ip_addresses.yml' and you can see this is looping over all the interfaces that we want to assign IP addresses for including the new RVI interfaces we just set up on the switch.

And that brings the whole IPAM piece for the new Brisbane site, together down to the individual IP address.

So, flip back to the web interface and check the results - starting with the prefixes, the supernet of 192.168.0.0/22 now has 6 child prefixes, and if you click on 192.168.2.0/26 for example this is network management prefix. This is assigned to the NETMAN VLAN 50, in the Brisbane_VLANS group. There are 6 IP addresses in use and you click the first one it has been assigned to interface me0 on the switch.

if you click the device name and then interfaces. you also see see the IP address that was assigned to interface ge-0/0/0, then if you click on page 2 of the results you can see the other IP address assignments including the ones to the newly created virtual routed interfaces for each vlan.

So finally, click back on the home page and now you san see that there are now 3 aggregates, 7 prefixes, 12 IP addresses and 6 VLANS.

So, I hope that has been a useful overview of how populate IPAM data using the Ansible Galaxy collection for NetBox. Without diving too deep into Ansible (this a netbox course after all) I hope you can see how easy it is to get started with Ansible and also how it can be integrated with NetBox.

This kind of integration opens up so many possibilities for what you can do with NetBox as your single source of truth for your network automation efforts.

So once again, thanks for watching!