Hello and welcome to this video for module 3 of the NetBox Zero to Hero training course. If you haven't already checked out the earlier modules yet then you can find the link to them in the notes below to get started.

For this demo I am using a NetBox Cloud instance. If you would like to follow along with the demo, then you can easily do that too. There are a couple of links down below to help you get set up with your own instance of NetBox along with a link to the notes that accompany this video module.

In this video, our intrepid Network Engineer Eric will be adding the network devices that are going to be installed at the planned new Brisbane branch office, making use of the NetBox REST API.

### Setting up the API Token
Ok so I am logged into NetBox as Eric, and as Eric is going to be using the REST API to add devices, he needs to set up his API Token. The REST API employs token-based authentication, which maps API clients to user accounts and their assigned permissions.

To set up the token click on Eric's username and then API tokens, then click Add a token. If no key is provided, one will be generated automatically. If you wanted to make the token read-only then you uncheck the 'write enabled' box, you can optionally set an expiry date and restrict access to the API from only certain IP addresses. so click on create and there we have Eric's API token ready to use.

So, click on copy and then we are going to switch to the Postman application. So in postman we already have a collection of API calls for NetBox set up ready to go. So in the collection, click variables and then add a new variable called api_token, paste in eric's token and save. Notice that we have a few other variables set up here in the collection too. If you need a re-cap on using variables in Postman then check out module 1 of this course where this was covered in more detail.

OK, so now we can make use the api token in our API calls. As you know Postman is an amazing tool to help you work with API's and by building a collection of API call's here, you not only learn how the API works, but you can also simply use the collection to interact with NetBox if you chose to.

### Add The Manufacturers
Before we can add any device types we need to add the manufacturers - so the first API call is a POST request to the dcim/manufacturers end point. Click in the Headers section to see the api token variable being used, and in the body of the request there is a list of json objects that represent each of the manufacturers - and this is how to can create multiple objects with a single API call.

So in the list are the names and slugs for Cisco, Cisco Meraki, Juniper, Panduit and Avocent.

OK, Click send, and the response is a 201 - so that's great, and we can see the response contains a list of the newly created manufacturer instances. Note also, the value in the ID field - each object has a numerical value for ID to identify it in the NetBox database. Remember if you are following along, your object ID's will most likely be different to the one's in this demo video.

Great - so switch back to the UI and - and there is the list of manufacturers. Note that we also have HPE and Vertiv already in the list as we added these manually in module 2 for our racks. So click on Cisco for example and once again note the ID is referenced in the top right corner of the page. (click back on manufacturers)

### Device Roles
So next Eric needs to add the Device Roles - so back in the postman collection there is an API call for this which is again a post request, this time to the dcim/device-roles api end point. This time in the body of the request we have a list of json objects representing each role starting with the WAN Router.

Again there is the name and slug, plus a hexadecimal value for a colour for the device role - feel free to pick whichever colours work for you. You do need to define whether this is a VM role or not and as these are network devices then set this to false.

So the other roles are access switch, wireless access point, patch panel and console server. Once again click send to make the call, and we have another successful request with a 201 response message back from the NetBox server, and the response contains a list of the newly created device role instances.

Now you could switch back to check this in the Web interface, but instead why not make use of another API call do this - there is a GET request on the collection that makes a call to the device roles API end point once again.

Note that this request making use of the 'brief' format by appending ?brief=1 onto the end of the request.

(click send)

This returns only a minimal representation of each object in the response. This is useful when you need only a list of available objects without any related data. For example you might develop an application that populates a drop-down list in a form before making an API call to NetBox.

### Platforms
OK, staying in postman, next add the platforms - again there is a POST request in the collection - this time to the dcim/platforms end point and the body of the request has a list of json objects representing the 2 network Operating Systems - Cisco IOS and Juniper Junos.

Click send again, and it's another successful request with a 201 response message, and the response contains a list of the newly created platforms. Once again note there is a numeric ID for the each of the platform objects that have been created - these numeric ID'd are very important for future API calls.

### Device Types
Next, it's time to add the device types, and a great way to do this is leveraging the amazing work of the contributors to the NetBox community device type library which you can find on Github in the link below this video and in the course notes.

(show https://github.com/netbox-community)

So if you go to the main NetBox Community repo, and click the link to the device-type library, there is a ready made collection of community-sourced DeviceType definitions for import to NetBox - which is huge time saver! This is another great example of why the netbox open source community is so amazing!

As the readme file states - this contains a set of device type definitions expressed in YAML and arranged by manufacturer. Each file represents a discrete physical device type (e.g. make and model). These definitions can be loaded into NetBox instead of creating new device type definitions manually.

So you can find links to copies of the yaml files for the device types needed for the new Brisbane office in the course notes. For example this is the YAML file for the Cisco ISR4321 router. Yaml files are very human readable and you can see all the properties of this particular device type clearly defined even down to the console and power ports!

If we look at the Meraki MR56 access point definition - this one even has a link to the datasheet in the comments.

so to add these into NetBox you import them via the Web interface. so go to device types and click import - paste in the yaml formatted data for the the ISR4321 router. then click submit. Next do the same for the Juniper EX4300 switch........then the Cisco Meraki MR56 access point........followed by the Avocent console server.......and lastly the Panduit 48 port patch panel - (click submit).

Great - so click Device types now and here is the full list - click the Cisco ISR4321 for example and you can see the main device type and the component templates for the interfaces.....the console ports and the power ports.

Check the MR56 AP and in the comments there is the link to the datasheet. Awesome!


**PAUSE HERE TO UPDATE OBJECT ID'S**


### Devices
Now it's time to add the devices as instances of the device types you just added, so flip back over to postman to do this via the REST API. The request type once again is POST and the API endpoint is dcim/devices, and as usual there is a list of json objects for each device.

So, the first device in the list is the WAN router, and the name is AUBRI01-RTR-1 so that's easy enough, but notice there are some fields like device type for example that have the numeric ID of the device type being instantiated - so the question is how do you know what that ID value is? well you could find it in the web interface as we have seen, but we could also make an API call to get the same value.

For example if you run the 'get device types' request you could return a list of them all like this (with ?brief=1), or you could filter the results to only return the data you need - for example to filter for the ISR4321 router model you could append '?model__ic=ISR' to the end of the request - hit send and there is the result containing the ID value of 6.

using the 'double underscore ic' lookup expression filters string fields (you can find much more on this in the NetBox documentation (show https://netboxlabs.com/docs/netbox/en/stable/reference/filtering/) in this case the 'double underscore ic' filter matches when a string contains the text, and is not case sensitive.

So you can add filters in this way to find all of the other ID values required to add the devices For example to find the ID of the location you want to install a device into - you could use the 'get locations' request and filter on a status of planned - and that will return only the new Brisbane location.

Some example requests using different filters are included in the Postman collection included with this course to get you started.

So - back to the add device request - there are some other fields that you can have values for - so in this case the device will be added to position 20 in the rack, it is facing the front, it has with front to rear airflow and the status is planned.

So, Click send, and the response is a 201 - so that's great, and the response contains a list of the newly created device instances.

Great stuff - now flip back to the web interface, and click devices and there is a list of the newly added devices! click into one of them (AUBRI01-SW-1) and notice how all the components are there, and you can click on the link to the rack elevation and then you can see it's location in the rack also - and you can see the space utilization of the rack now too with all the devices installed in it.

So, I hope that has been a useful overview of how to add devices into NetBox using the REST API and Postman, and hopefully you had fun following along on your own NetBox instance! Thanks for watching.