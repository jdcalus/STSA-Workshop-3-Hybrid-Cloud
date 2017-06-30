# Workshop 3 Hybrid Heterogeneous Cloud

## Introduction
In this workshop, we’ll work with APIs through Node-RED to use and create services for a weather preparedness situation. 

The fictitious company we’re working with is a large retail company that centralizes their core business assets, but still utilizes public cloud APIs to extend their functionality. At the heart of operations is a large key/value store system, which holds information about products as well as inventory information for each store. 

This lab uses Node-RED, and you can either use an existing instance, or create a new one. Either way, make note of the instance name so you can easily reference the web page and ReST interfaces. 

You may also wish to use a ReST client to test the APIs, and view debug data. Some popular ReST clients include RESTClient for Firefox (http://restclient.net/) and Restlet Client for Chrome (https://restlet.com/modules/client/)

Using a ReST client is typically done by specifying the HTTP directive (a GET, which typically requests information, for example) and an address, like this:

![Architecture Overview](/images/rest_client.png)


The important fields to note here are the Method, URL, Response, and Body. The Method is the HTTP verb that is being performed against the HTTP endpoint. For our examples here, we will only be reading data, so we will be performing a GET. For the curious, a POST is typically used to create new objects, PUT is used to update existing objects, and DELETE is used to delete existing objects. There are more verbs, but these are the main four. 

Next is the URL, which represents the object we are acting upon. If it looks like a website url, that’s because ReST-ful APIs use HTTP mechanisms to facilitate communication, and that’s essentially what it is; a web address. It probably won’t work if you put it into a web browser, though, it is meant to be called from a client or code that can handle the input and output correctly. 

This familiarity is what makes it so easy for just about any programming language to support ReST-ful APIs without the need for specific libraries and connectors. As long as someone’s program can reach out to a web server, it can initiate an API. 

The Response is simply the output from the web service. If issuing a GET, this is where you’ll find the requested object. For POST, PUT, and DELETEs, you will most likely only get the response code. Just like HTTP, 200 means OK, everything worked correctly, though some services will pass back additional information that may be helpful to the creator. 

When making a POST or PUT, we have to send along the data to be created or updated. This is where we use the Body of a ReST call. For this lab, we will not be making POSTs or PUTs, so we won’t have to worry about this. 

## Exercise 1:

We need to begin by putting the starter code into a new Node-RED workspace. This can be done by using the menu button on the top right corner of Node-RED, and then selecting Import -> Clipboard. Paste the code in here and click Import. 
```JSON
[
    {
        "id": "4b7c1d6d.b4b674",
        "type": "http in",
        "z": "e254368b.f68d08",
        "name": "Item Lookup",
        "url": "/itemlookup",
        "method": "get",
        "swaggerDoc": "",
        "x": 368,
        "y": 220,
        "wires": [
            [
                "a7b0c002.74dc4"
            ]
        ]
    },
    {
        "id": "ef0e5b47.cd9698",
        "type": "http response",
        "z": "e254368b.f68d08",
        "name": "Return ItemID",
        "x": 1125,
        "y": 229.25,
        "wires": []
    },
    {
        "id": "a7b0c002.74dc4",
        "type": "http request",
        "z": "e254368b.f68d08",
        "name": "Company Item Lookup",
        "method": "GET",
        "ret": "obj",
        "url": "http://mvs1.centers.ihost.com:50200/resources/ecs/IBM/demo/{{{payload.name}}}",
        "tls": "",
        "x": 703.5,
        "y": 227.75,
        "wires": [
            [
                "ef0e5b47.cd9698",
                "cbea68c.b024898"
            ]
        ]
    },
    {
        "id": "fde5a384.3028a",
        "type": "http in",
        "z": "e254368b.f68d08",
        "name": "Inventory Lookup",
        "url": "/inventorylookup",
        "method": "get",
        "swaggerDoc": "",
        "x": 375,
        "y": 296,
        "wires": [
            [
                "3018569e.e3aeda"
            ]
        ]
    },
    {
        "id": "7a36e7ac.6eb908",
        "type": "http response",
        "z": "e254368b.f68d08",
        "name": "Return Store Inventory",
        "x": 1147,
        "y": 297.25,
        "wires": []
    },
    {
        "id": "3018569e.e3aeda",
        "type": "http request",
        "z": "e254368b.f68d08",
        "name": "Store Lookup",
        "method": "GET",
        "ret": "obj",
        "url": "",
        "tls": "",
        "x": 716.5,
        "y": 292.75,
        "wires": [
            [
                "7a36e7ac.6eb908",
                "cbea68c.b024898"
            ]
        ]
    },
    {
        "id": "cbea68c.b024898",
        "type": "debug",
        "z": "e254368b.f68d08",
        "name": "CICS Replies",
        "active": true,
        "console": "false",
        "complete": "payload",
        "x": 934.5,
        "y": 261.5,
        "wires": []
    },
    {
        "id": "7cc888be.da9b28",
        "type": "inject",
        "z": "e254368b.f68d08",
        "name": "Test Store Lookup",
        "topic": "",
        "payload": "{\"itemnumber\":\"78707740\",\"storeprefix\":\"sample_store\"}",
        "payloadType": "str",
        "repeat": "",
        "crontab": "",
        "once": false,
        "x": 368.5,
        "y": 406.25,
        "wires": [
            [
                "8880be2d.2826c"
            ]
        ]
    },
    {
        "id": "8880be2d.2826c",
        "type": "json",
        "z": "e254368b.f68d08",
        "name": "",
        "x": 584.5,
        "y": 400.75,
        "wires": [
            [
                "3018569e.e3aeda"
            ]
        ]
    },
    {
        "id": "aa8be08.809392",
        "type": "inject",
        "z": "e254368b.f68d08",
        "name": "Test Item Lookup (by name)",
        "topic": "",
        "payload": "{\"name\":\"bucket\"}",
        "payloadType": "str",
        "repeat": "",
        "crontab": "",
        "once": false,
        "x": 399,
        "y": 363,
        "wires": [
            [
                "e028af17.d2f07"
            ]
        ]
    },
    {
        "id": "e028af17.d2f07",
        "type": "json",
        "z": "e254368b.f68d08",
        "name": "",
        "x": 589,
        "y": 361.5,
        "wires": [
            [
                "a7b0c002.74dc4"
            ]
        ]
    }
]
```

![Architecture Overview](/images/import_code.png)


We have two very simple services here. In fact, they're so simple, they do very little beyond what the service they're calling does. We'll start here, however, just so you get a basic understanding of using services to build functionality.

![Architecture Overview](/images/first_two_flows.png)


There are two types of boxes that will help you debug your code throughout this lab; 
the Inject box and the Debug box. Clicking the square on the left side of a light-blue 
Inject box sends a specific piece of data into a flow. This can be used to emulate a web 
call or device input, and can only be used as input sources. The dark green node, called 
the Debug node, grabs the values of a particular object and displays it on the Debug panel 
on the right side of your screen. Debug nodes can be temporarily deactivated by clicking on 
the green square on the right side.   

![Architecture Overview](/images/inject_debug.png)

You can try this yourself by dragging each of the nodes into the flow.

Double click on the **Inject** node and replace the values as follows:
![Architecture Overview](/images/inject-hello.png)

Double click on the debug node and change the name:
![Architecture Overview](/images/debug-hello.png)

- Now click on the **Deploy** button at the top of the screen.
		You can now click on the left side of the "Hello World" and you should see the output in the debug panel
		![Architecture Overview](/images/hello-debug-panel.png)		

The first line is a service, and should already work. You can test it and view the debug output using those Inject and Debug nodes.   
- Click on the inject button for the "Test Item Lookup" 

![Architecture Overview](/images/inject-test-item-lookup-node.png)

Your output in the debug panel should look like:

![Architecture Overview](/images/test-item-lookup-debug.png)


The flow accepts a GET request, and uses the information in the GET querystring to perform its own GET request to the inventory key/value store, which is running out of CICS. It then simply takes the response and sends it back out to the original requestor. By opening the node for the HTTP call, you can see how the item name is substituted in the url by use of triple curly braces. {{{like.this}}} 

![Architecture Overview](/images/http_request.png)




- Do the same thing for the "Test Store Lookup"

What you see is probably not what you expect.
![Architecture Overview](/images/store-lookup-debug.png)
As you can see in the debug panel there is an error "No url specified". This is because the "HTTP Node" is not configured properly.

- Double click on the **Store Lookup"** node
![Architecture Overview](/images/http-store-lookup-node.png)

This intentionally doesn't work so we need to fix it. The "URL" field was left empty.
![Architecture Overview](/images/http-store-lookup-node-edit.png)


The HTTP IN and OUT are correct, but this service performs a slightly different query.

The company uses this service to check on the status of a particular item in a specific store, and they use the following format:

http://[hostname]]/resources/ecs/IBM/demo/[store prefix]_[item number]

So, for example, if we wanted to look up the status of an item in the Poughkeepsie, NY store location, we would use the following key:


`http://mvs1.centers.ihost.com:50200/resources/ecs/IBM/demo/{{{payload.storeprefix}}}_{{{payload.itemnumber}}}`

![Architecture Overview](/images/http-store-lookup-node-edit-url.png)


Your HTTP REST call should look fairly similar to the first one, except that you'll be using the triple-curly-braces to substitute two parameters instead of one. Remember the formatting above.

When done, you should have two services, and both Test buttons should return something looking like item information from the CICS Replies debug node.

Tip: Don't worry if you see a "No Response Object" warning while testing with the buttons. Since we're calling it from a manual inject, and not an HTTP request, the HTTP response node gets confused, but it'll work just fine when we call it as a real service. 

You can also drive these services from a REST client, by using the addresses noted in the HTTP In nodes.


 
## Exercise 2:
So far, we've just created two simple services, and they don't do much on their own, but it's time to make a service that provides a little more value.

A fairly common request of a developer would probably be to have an easy way to translate between product names and numbers. We can actually do this in the same service by leveraging what we have already created above.

Copy and paste the code for Exercise #2 below your existing services and take a look at the nodes.

```JSON
[
    {
        "id": "1fe6e497.2346db",
        "type": "switch",
        "z": "268172cf.50649e",
        "name": "IsNumerical",
        "property": "payload",
        "propertyType": "msg",
        "rules": [
            {
                "t": "regex",
                "v": "[0-9999999999999999]",
                "vt": "str",
                "case": false
            },
            {
                "t": "else"
            }
        ],
        "checkall": "true",
        "outputs": 2,
        "x": 739,
        "y": 205.5,
        "wires": [
            [],
            []
        ]
    },
    {
        "id": "ad366617.b6add8",
        "type": "inject",
        "z": "268172cf.50649e",
        "name": "generator",
        "topic": "",
        "payload": "generator",
        "payloadType": "str",
        "repeat": "",
        "crontab": "",
        "once": false,
        "x": 218,
        "y": 189,
        "wires": [
            [
                "a76f9ae5.9f9308"
            ]
        ]
    },
    {
        "id": "cb215e88.77d8",
        "type": "http in",
        "z": "268172cf.50649e",
        "name": "Translate Name/Number",
        "url": "/translate",
        "method": "get",
        "swaggerDoc": "",
        "x": 196.5,
        "y": 244.75,
        "wires": [
            [
                "a76f9ae5.9f9308"
            ]
        ]
    },
    {
        "id": "110ad224.ce393e",
        "type": "http response",
        "z": "268172cf.50649e",
        "name": "Return Translated Name/Number",
        "x": 991.5,
        "y": 399.75,
        "wires": []
    },
    {
        "id": "b42dfadd.897768",
        "type": "comment",
        "z": "268172cf.50649e",
        "name": "Look up ItemID to get the Name",
        "info": "",
        "x": 283.7499694824219,
        "y": 394.25006103515625,
        "wires": []
    },
    {
        "id": "1c802434.2fb90c",
        "type": "comment",
        "z": "268172cf.50649e",
        "name": "Look up the Name to get the Itemid",
        "info": "",
        "x": 284,
        "y": 553.5,
        "wires": []
    },
    {
        "id": "54cd85ab.f5af9c",
        "type": "comment",
        "z": "268172cf.50649e",
        "name": "Return the appropriate value to the HTTP Out node",
        "info": "",
        "x": 1028,
        "y": 445.25006103515625,
        "wires": []
    },
    {
        "id": "a76f9ae5.9f9308",
        "type": "function",
        "z": "268172cf.50649e",
        "name": "Set store_prefix",
        "func": "msg.store_prefix = \"sample_store\";\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 521,
        "y": 206.5,
        "wires": [
            [
                "1fe6e497.2346db"
            ]
        ]
    },
    {
        "id": "62c88ddd.e88754",
        "type": "function",
        "z": "268172cf.50649e",
        "name": "Parse out Itemid",
        "func": "msg.payload = msg.payload.itemid;\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 523,
        "y": 584.5,
        "wires": [
            [
                "110ad224.ce393e",
                "63612431.7ce00c"
            ]
        ]
    },
    {
        "id": "63612431.7ce00c",
        "type": "debug",
        "z": "268172cf.50649e",
        "name": "Translate Debug",
        "active": false,
        "console": "false",
        "complete": "payload",
        "x": 936,
        "y": 366.25,
        "wires": []
    },
    {
        "id": "f18dea26.3b1238",
        "type": "inject",
        "z": "268172cf.50649e",
        "name": "19645922",
        "topic": "",
        "payload": "19645922",
        "payloadType": "str",
        "repeat": "",
        "crontab": "",
        "once": false,
        "x": 207.5,
        "y": 296.75,
        "wires": [
            [
                "a76f9ae5.9f9308"
            ]
        ]
    }
]
```

![Architecture Overview](/images/exercise-two-initial-view.png)


Following the flow from the beginning, we've got an HTTP In (Translate Name/Number), so this can be called as an API. 
There are also two Test Inject buttons (generator) and (19645922) which you can use to easily debug your code.
The basic princple of this API is to allow the user to send an "itemID" and get a "product name" in response or

If you double click on the "Set store prefix" 
![Architecture Overview](/images/set-store-prefix-node.png)

You will see that the we set the "msg.store_prefix" attribute of the JSON object to "sample_store"
![Architecture Overview](/images/set-store-prefix-node-details.png)

We are using  "sample_store" as our store prefix, because data is already populated on zOS for this store prefix. 

The "IsNumerical" node uses Regex to figure out if the incoming payload is text or numerical. 
If it's text (an item name), then we'll need to use the same logic we did above to get the ItemId.

The "isNumerical" node is a **switch** node which allows for the flow to change based on the conditions defined in the switch node details.
![Architecture Overview](/images/isnumerical-node.png)

As you can see in the image below where are two conditions in he "switch node".
![Architecture Overview](/images/isnumerical-node-details.png)

The first condition evaluated is that a number value between 0 and 9999999 is stored in msg.payload.
The second condition is the catch all condition, which in our case says, that if the values in msg.payload are not a number we wil pass it here.

Now look at the isNumerical node again, you will see two small gray circles on the right of the node.
![Architecture Overview](/images/isnumerical-node.png)
Those two circles correspond to the two condition in the node details. The top circle is used for the number matching and the bottom circle is for the catch all.

Remember from the prior exercise we showed an API that looked like the following:
`http://[hostname]]/resources/ecs/IBM/demo/[store prefix]_[item number]`

Because the "isNumerical" is passing a number and the "set store_prefix" is setting the store name we have both pieces of information we need for the API.

The JSON object that gets created from the "set store_prefix" and from the "inject" nodes is the following:

```JSON
{
"store_prefix":"sample_store",
"payload":"19645922"
}
``` 

So now we want to create the URL by using the "Mustache" syntax.
`http://[hostname]]/resources/ecs/IBM/demo/{{{store_prefix}}}_{{{payload}}}`

This will take the "store_prefix" JSON value and the "payload" JSON value and add it to the end of the URL. We now need to add the http request to invoke the URL.

Drag the **http request** node from the pallet to the flow editor.

![Architecture Overview](/images/http-request-node.png)

 Now **double click** the node. 

![Architecture Overview](/images/http-request-editor-node.png)


Change the name of the node to "item lookup".

![Architecture Overview](/images/item-lookup-node.png)

Also paste the following url into the URL field.
`http://mvs1.centers.ihost.com:50200/resources/ecs/IBM/demo/{{{store_prefix}}}_{{{payload}}}` 
![Architecture Overview](/images/item-lookup-detail.png)

Make sure to change the "Return" value from "a UTF-8 string" to "a parsed JSON object"

Close the detail view of the "item Lookup" node. Create a connection between the "Item lookup" and "Translate Debug" nodes. This will allow us to see what is returned from invoking the API.
![Architecture Overview](/images/connect-itemid-debug-nodes.png)

Click the **Deploy** button at the top of the screen.

![Architecture Overview](/images/deploy-button.png)

This will push your updates to the server on BlueMix.


Next, click on the left side of the **Inject** node that has "19645922". This causes the inject to be fired and the flow to be executed.

![Architecture Overview](/images/inject-itemid-node.png)

You should see something like the following in the debug window on the right side:
![Architecture Overview](/images/item-lookup-debug-output.png)

Okay, as you can see there is a JSON result coming from the API call.
```JSON
{
	"inventory":24,
	"price":"49.98",
	"name":"Rainjacket",
	"min_stock":35,
	"location_code":"466.014.812"
}
```

Remember our newly create REST API is supposed to only return the "Name" of a product when supplied with a "ItemID". So we need to clean up the JSON response to only return the "Name" value. To do this we need to drag the "Function" node from the pallet to the editor.

![Architecture Overview](/images/function-node-pallet.png)

Next delete the link between the "Item Lookup" node and the "Tranlate Debug" node. Create a new connection between the "Item Lookup" and the newly dropped "Function" node. Change the name of the "Function" node to "Parse out Name". This is done by double clicking on the "Function" node and changing the name. Your function node editor view should look like:

![Architecture Overview](/images/parse-out-name-node.png)

Make sure to paste the following code into the function editor

```Javascript
	msg.payload = msg.payload.name;
	return msg;
```

Click **Done** when finished. 


The above code is taking the results "msg.payload.name" from the JSON that was returned from the API call and reassigning the "msg.payload" with the name value. This is because our newly created REST API is only supposed to return a name, if the itemID is supplied.

Now connect the "Parse out Name" function to the "Translate Debug" node and to the "Return Translated Name/Number" http response node.

![Architecture Overview](/images/parse-out-name-flow.png)

Click the **Deploy** button at the top of the screen.

![Architecture Overview](/images/deploy-button.png)

This will push your updates to the server on BlueMix.

Next, click on the left side of the **Inject** node that has "19645922". This causes the inject to be fired and the flow to be executed.

![Architecture Overview](/images/inject-itemid-node.png)

You should see something like the following in the debug window on the right side:

![Architecture Overview](/images/parse-out-name-debug.png)

As you can see the response is just the name now, not the rest of the JSON object.
Don't worry about the "No response object" message shows.


Next we are going to do the same thing as we just did, but now for translating a "product name" into a "itemID".


Drag the **http request** node from the pallet to the flow editor.

![Architecture Overview](/images/http-request-node.png)

 Now **double click** the node. 

![Architecture Overview](/images/http-request-editor-node.png)


Change the name of the node to "name lookup" and add the following URL:
``http://mvs1.centers.ihost.com:50200/resources/ecs/IBM/demo/{{{payload}}}``

Notice in this URL the only attribute we are looking for is the "payload". The store number isn't needed. Aslo, make sure to change the "Return" value from "a UTF-8 string" to "a parsed JSON object".

Click **Done** when finished.

Now connect the "name lookup" to the "Parse out Itemid". You are just about done. Click the **Deploy** button at the top of the screen.

![Architecture Overview](/images/deploy-button.png)

Now click on the **generator** inject node.

![Architecture Overview](/images/inject-generator-node.png)

You should now see something like the following in the debug panel on the right side of the page:

![Architecture Overview](/images/generator-flow-debug.png)

Okay your final flow should look something like the following:

![Architecture Overview](/images/exercise-two-full-flow.png)

Congrats you have completed exercise 2. The completed new service will make it easy for a developer to translate back and forth between item names and numbers.

## Exercise 3:
Start a new tab in Node-RED, and paste in the nodes for Exercise 3. 
```JSON
[
    {
        "id": "ca451b94.dfcd38",
        "type": "http request",
        "z": "69456808.2e1bb8",
        "name": "Name to Number",
        "method": "GET",
        "ret": "obj",
        "url": "https://stsa-retail.mybluemix.net/itemlookup?name={{{payload}}}",
        "tls": "",
        "x": 647,
        "y": 620.7499389648438,
        "wires": [
            [
                "e3ddd8d.3103d28"
            ]
        ]
    },
    {
        "id": "e3ddd8d.3103d28",
        "type": "http request",
        "z": "69456808.2e1bb8",
        "name": "Get Local Store Info",
        "method": "GET",
        "ret": "obj",
        "url": "https://stsa-retail.mybluemix.net/inventorylookup?storeprefix=sample_store&itemnumber={{{payload.itemid}}}",
        "tls": "",
        "x": 892.7500610351562,
        "y": 620,
        "wires": [
            [
                "cf4eda7a.106418"
            ]
        ]
    },
    {
        "id": "25b38008.1d8e5",
        "type": "http request",
        "z": "69456808.2e1bb8",
        "name": "",
        "method": "GET",
        "ret": "obj",
        "url": "https://twcservice.mybluemix.net/api/weather/v1/geocode/{{{payload}}}/forecast/hourly/48hour.json",
        "tls": "",
        "x": 371.2738342285156,
        "y": 390.63092041015625,
        "wires": [
            [
                "d7e2d42.840c528",
                "87ac5c80.2362"
            ]
        ]
    },
    {
        "id": "aad4cbe.45d1e38",
        "type": "function",
        "z": "69456808.2e1bb8",
        "name": "Format Data",
        "func": "msg.lat = msg.payload.location.latitude[0]; \nmsg.long = msg.payload.location.longitude[0]; \nmsg.payload = msg.lat+\"/\"+msg.long; \n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 869.4166870117188,
        "y": 320.05950927734375,
        "wires": [
            [
                "25b38008.1d8e5"
            ]
        ]
    },
    {
        "id": "34e2595c.2a54e6",
        "type": "inject",
        "z": "69456808.2e1bb8",
        "name": "",
        "topic": "",
        "payload": "{\"search\":\"Des Moines\"}",
        "payloadType": "str",
        "repeat": "",
        "crontab": "",
        "once": false,
        "x": 258.75,
        "y": 453.14306640625,
        "wires": [
            [
                "2791472a.1968f8"
            ]
        ]
    },
    {
        "id": "d7e2d42.840c528",
        "type": "function",
        "z": "69456808.2e1bb8",
        "name": "Save PoP",
        "func": "msg.pop = msg.payload.forecasts[0].pop;\nreturn msg",
        "outputs": 1,
        "noerr": 0,
        "x": 626,
        "y": 395.5,
        "wires": [
            [
                "7983d8a1.0a4588",
                "bd78c800.fc88b8"
            ]
        ]
    },
    {
        "id": "8dc72856.ec6cf8",
        "type": "http in",
        "z": "69456808.2e1bb8",
        "name": "",
        "url": "/weather",
        "method": "get",
        "swaggerDoc": "",
        "x": 210.75,
        "y": 139.5,
        "wires": [
            [
                "e32210d0.48d69"
            ]
        ]
    },
    {
        "id": "1967294a.30fb37",
        "type": "http response",
        "z": "69456808.2e1bb8",
        "name": "Web Response",
        "x": 864.9999389648438,
        "y": 1166,
        "wires": []
    },
    {
        "id": "a9f75045.238da",
        "type": "template",
        "z": "69456808.2e1bb8",
        "name": "Template",
        "field": "payload",
        "fieldType": "msg",
        "format": "handlebars",
        "syntax": "mustache",
        "template": "<!DOCTYPE html>\n<html>\n<head>\n\t<title>Store Inventory</title>\n</head>\n<style>\n    body {\n        background-color: #26343F;\n    }\n    .main {\n        background-size: 100%;\n        background-repeat: repeat-x;\n        background: linear-gradient(to top right, rgba(29, 54, 73, 1), rgba(45, 63, 74, 0.98)), url(http://roadshow.mybluemix.net/images/i-l-pattern.png);\n    }\n    .logo {\n\t    font-size:16px; \n\t    color: white;\n\t    font-family: sans-serif;\n\t    margin-top:12px; \n\t    padding-left:10px; \n\t    margin-bottom:15px;\n\t    display:inline-block;\n\t}\n\t.title {\n\t    font-size:48px; \n\t    color: white;\n\t    font-family: sans-serif;\n\t    margin-top:50px; \n\t    padding-left:90px; \n\t    margin-bottom:0px;\n\t    display:inline-block;\n\t}\n    .sub-title {\n\t    font-size:24px; \n\t    color: #41D6C3;\n\t    font-family: sans-serif;\n\t    margin-top:10px; \n\t    padding-left:90px; \n\t    margin-bottom:0px;\n\t    display:block;\n\t}\n</style>\n<body>\n\t<span class=\"logo\" >IBM <b>Bluemix</b></span>\n\t<section class=\"main\">\n\t\t<div class=\"title\">Weather Preparedness Portal</div>\n\t\t<div class=\"sub-title\"><div>\n\t\t{{{formatted_items}}}\n\t</section>\n\t<span></span>\n</body>\n</html>",
        "x": 481,
        "y": 1167.25,
        "wires": [
            [
                "facb5ba1.570c98"
            ]
        ]
    },
    {
        "id": "3e95365a.6ba1ba",
        "type": "split",
        "z": "69456808.2e1bb8",
        "name": "",
        "splt": ",",
        "x": 471.25,
        "y": 620,
        "wires": [
            [
                "ca451b94.dfcd38"
            ]
        ]
    },
    {
        "id": "f014283e.332628",
        "type": "join",
        "z": "69456808.2e1bb8",
        "name": "",
        "mode": "custom",
        "build": "string",
        "property": "payload",
        "propertyType": "msg",
        "key": "topic",
        "joiner": "",
        "timeout": "2",
        "count": "",
        "x": 460,
        "y": 908.2500610351562,
        "wires": [
            [
                "2ca3b409.6b3a7c"
            ]
        ]
    },
    {
        "id": "facb5ba1.570c98",
        "type": "function",
        "z": "69456808.2e1bb8",
        "name": "header",
        "func": "msg.headers = {'Content-Type': 'text/html'};\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 664.0000610351562,
        "y": 1172.75,
        "wires": [
            [
                "1967294a.30fb37"
            ]
        ]
    },
    {
        "id": "cf4eda7a.106418",
        "type": "function",
        "z": "69456808.2e1bb8",
        "name": "Iterate and Format",
        "func": "msg.inventory = msg.payload;\n   msg.payload = \"<p>\";\n   msg.payload += msg.inventory.name\n   msg.payload += \" (\"+msg.inventory.inventory+\")\"\nreturn msg\n\n",
        "outputs": 1,
        "noerr": 0,
        "x": 359,
        "y": 690.75,
        "wires": [
            [
                "dca8e1e1.747ff",
                "f014283e.332628"
            ]
        ]
    },
    {
        "id": "2ca3b409.6b3a7c",
        "type": "function",
        "z": "69456808.2e1bb8",
        "name": "Formatted",
        "func": "msg.formatted_items = msg.payload;\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 638.25,
        "y": 909.75,
        "wires": [
            [
                "a9f75045.238da"
            ]
        ]
    },
    {
        "id": "e32210d0.48d69",
        "type": "function",
        "z": "69456808.2e1bb8",
        "name": "Dallas Search",
        "func": "msg.city = {\"search\":\"Dallas\"}\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 220.5,
        "y": 318.4999694824219,
        "wires": [
            [
                "f82a682c.818698"
            ]
        ]
    },
    {
        "id": "bd78c800.fc88b8",
        "type": "debug",
        "z": "69456808.2e1bb8",
        "name": "",
        "active": false,
        "console": "false",
        "complete": "pop",
        "x": 873.75,
        "y": 386,
        "wires": []
    },
    {
        "id": "84d7d8c7.202798",
        "type": "comment",
        "z": "69456808.2e1bb8",
        "name": "HTTP In kicks off the flow with a comma-seperated list of weather-related items. ",
        "info": "",
        "x": 446.5,
        "y": 98,
        "wires": []
    },
    {
        "id": "98fc0a55.1f6e98",
        "type": "comment",
        "z": "69456808.2e1bb8",
        "name": "The list gets split out, each item gets checked in the store's inventory, formatted, and combined back together",
        "info": "",
        "x": 580.5,
        "y": 564.5,
        "wires": []
    },
    {
        "id": "22395582.8fd42a",
        "type": "comment",
        "z": "69456808.2e1bb8",
        "name": "Looking up the weather information for a specific city, we determine the likelyhood of rain (PoP)",
        "info": "",
        "x": 493.25,
        "y": 275.5,
        "wires": []
    },
    {
        "id": "f5d7c8b0.07f198",
        "type": "comment",
        "z": "69456808.2e1bb8",
        "name": "Build a web page and return it to the original HTTP requestor",
        "info": "",
        "x": 594,
        "y": 1123.25,
        "wires": []
    },
    {
        "id": "8ec6e0db.d9928",
        "type": "switch",
        "z": "69456808.2e1bb8",
        "name": "Check Reorder Level",
        "property": "inventory.inventory",
        "propertyType": "msg",
        "rules": [
            {
                "t": "else"
            },
            {
                "t": "eq",
                "v": "",
                "vt": "str"
            }
        ],
        "checkall": "true",
        "outputs": 2,
        "x": 923.5,
        "y": 703.2499389648438,
        "wires": [
            [],
            []
        ]
    },
    {
        "id": "b3f9ddae.947cb",
        "type": "function",
        "z": "69456808.2e1bb8",
        "name": "Add Rain Icon",
        "func": "msg.payload += \"<img src = \\\"http://mvs1.centers.ihost.com:50200/resources/ecs/IBM/demo/rain.png\\\">\"\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 1166,
        "y": 699.25,
        "wires": [
            [
                "f014283e.332628"
            ]
        ]
    },
    {
        "id": "7983d8a1.0a4588",
        "type": "function",
        "z": "69456808.2e1bb8",
        "name": "Weather Items",
        "func": "msg.payload = 'flashlight,bucket,boots,ducttape,generator'\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 310.5,
        "y": 618.75,
        "wires": [
            [
                "3e95365a.6ba1ba"
            ]
        ]
    },
    {
        "id": "87ac5c80.2362",
        "type": "debug",
        "z": "69456808.2e1bb8",
        "name": "",
        "active": false,
        "console": "false",
        "complete": "false",
        "x": 997.375,
        "y": 255.625,
        "wires": []
    },
    {
        "id": "2791472a.1968f8",
        "type": "http request",
        "z": "69456808.2e1bb8",
        "name": "",
        "method": "GET",
        "ret": "obj",
        "url": "https://twcservice.mybluemix.net/api/weather/v3/location/search?language=en_US&locationType=city&query={{payload.search}}",
        "tls": "",
        "x": 629.25,
        "y": 316.25,
        "wires": [
            [
                "aad4cbe.45d1e38"
            ]
        ]
    },
    {
        "id": "f82a682c.818698",
        "type": "json",
        "z": "69456808.2e1bb8",
        "name": "",
        "x": 413.625,
        "y": 320.9375,
        "wires": [
            [
                "2791472a.1968f8"
            ]
        ]
    },
    {
        "id": "dca8e1e1.747ff",
        "type": "debug",
        "z": "69456808.2e1bb8",
        "name": "",
        "active": false,
        "console": "false",
        "complete": "false",
        "x": 978.625,
        "y": 479.375,
        "wires": []
    },
    {
        "id": "4064c0d4.a7637",
        "type": "switch",
        "z": "69456808.2e1bb8",
        "name": "Check Weather Status",
        "property": "payload",
        "propertyType": "msg",
        "rules": [
            {
                "t": "eq",
                "v": "",
                "vt": "str"
            },
            {
                "t": "eq",
                "v": "",
                "vt": "str"
            }
        ],
        "checkall": "true",
        "outputs": 2,
        "x": 669.875,
        "y": 703.4375,
        "wires": [
            [],
            []
        ]
    }
]
```
This is a big one, and makes use of both the Weather Channel data, as well as the product information we have made available through our previous two exercises.

The first thing to do is get your weather data http requests working. By default (and with good reason!), Node-RED does not export user credentials when you import/export, so you’ll have to enter in your own credentials here. These are not your w3 credentials, but the username/password specific to the Weather Channel Data service, which you’ll create in the next step.

Go to bluemix.net and head over to Catalog, which will show you the list of services available. Find “Weather Company Data”, under Data & Analytics, and Create a new instance of this service, accepting the defaults. On the left-hand side, find “Service Credentials”and then “View Credentials”, which is where you’ll find the username/password. Enter this information into both http request nodes for the weather section of this flow.

![Architecture Overview](/images/credentials.png)



With your credentials entered, you should be able to access a web page by heading to [name].mybluemix.net/weather.

It will return a list of weather-related items.
Note: You'll note that the page returns in a different order each time. This is because calls made after the "Split" node are being made asynchronously and returned as soon as they complete. If order were important, we could write a function to order them.

Right now, the page is only showing five items, but the inventory actually has ten weather-related items. Update the page so it shows information for all of them. The keys for these items are:
umbrella, generator, rainjacket, flashlight, batteries, bucket, sumppump, boots, ducttape, water.

## Exercise 4:
We need to highlight the weather-related items that are below recommended inventory levels prior to a rainy day. You may notice the "Check Weather Status" and "Check Reorder Status" nodes, which are Switch statements. Use these to determine first, if the probability of rain is above a certain value (your choosing), and if so, then check to see if the current inventory is below the "Minimum Stock" value. If both of these evaluate to true, route the flow through the "Add Rain Icon" node, otherwise, continue the flow to the "Join" node below.
![Architecture Overview](/images/two_switch_nodes.png)



If you need help finding the values for these comparisons, check the various Function nodes where we moved values out of payload and into other objects. Also, if you're having trouble figuring out the correct keys to use for the store inventory, turn on the CICS debug node for the full output, which should contain some clues.

##Exercise 5:
You probably noticed there were a few fields in the key/value store that were not used, such as tags, and location code. There is also a ton of information returned from the Weather Channel Data service that we didn't use for this example. 

What are some ways the page, or the individual services, could be improved by taking into consideration this additional data? Or, using the data that we already have, what additional services could we provide to developers who are building their own applications?

What are some situations where you would want to update data as well as read it, and how might you go about implementing that in a Node-RED flow? 

