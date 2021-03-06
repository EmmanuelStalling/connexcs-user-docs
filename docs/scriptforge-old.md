# Table of Contents

* [Table of Contents](#table-of-contents)
* [ScriptForge](#scriptforge)
    * [Language](#language)
    * [Memory](#memory)
    * [CPU (Time Limit)](#cpu-time-limit)
    * [Durability](#durability)
    * [Scalability](#scalability)
    * [Functionality](#functionality)
    * [API](#api)
    * [Libraries](#libraries)
        * [Lookup](#lookup)
        * [Send](#send)
    * [Examples](#examples)
        * [Random CLI](#random-cli)
        * [Checking a DNC List](#checking-a-dnc-list)
        * [Performing an ASRPlus Lookup](#performing-an-asrplus-lookup)

# ScriptForge
The ScriptForge allows you to write your own application which can be run at specific events in the system, currently the following places support scripting functionality:

* Routing Engine (runs in line the call setup process)

Requests to your application are processed via a messaging bus and will be queued if your application can not process them fast enough. If you run your application in the routing engine you must ensure that it can complete quickly so as not to induce high PDD.

## Design Considerations

### Language

ScriptForge currently supports Java script (ECMA6)

### Memory

For speed enhancements variables in your application may be present between messages, however you should not assume this behaviour. Your application will be automatically distributed across multiple nodes and active variables will not be persistent (If you require persistence across messages you can use the datastore class). The memory of your application may be limited (default to 100MB). You should ensure that you don't try to use more than the allocated amount or your application may crash.

### CPU (Time Limit)

There are no hard CPU restrictions on your application, however its run time is limited (default 2 seconds).

The first message to be processed can take up to 300ms to instantiate, further messages (in proximity) will be processed near instantaneously.


### Durability

If a message is not processed in time or fails half way through processing, it will be reset to the unconsumed state and will be available for further processing. A message should not ever be lost.

### Scalability

Your application may run across multiple zones (data centres) and by default is allowed 1 process per script.

If your application is intensive and/or you need to scale further please contact us to arrange more resources to be dedicated to your application.

### Functionality

Your programming environment is locked down for security reasons, but we provide classes/helpers to achieve greater functionality, you can find out more about them in the Knowledge Base.

## API

Code will start in the `main()` function
Execution will be marked as complete when the `exit(response)` function is called, response is the variable that will be returned to the instantiating system.
If you wish to do the equivalent of throwing an exception, you can call the `err();` which takes a single parameter for example `[404, "Not Found"]`
Variables can be made available through the `vars()` function.

A basic routing application will look something like this:

```
function main(){
  var data = vars();
  exit(data);
}
```


ScriptForge as default is designed to process 1 concurrent execution per server. It is therefore important to execute `exit()` as soon as possible as the server will be blocking untill this operation completes. It is also important to ensure that there are no other branches of your code executing after the `exit()` has been called as this will slow down subsequent requests and could also cause unexpected termination & time outs of subsequent requests.

## Libraries
This section is lightly documented, if you have any questions please ask us.

### Lookup
All functions return a promise which contains an object with the same key values pairs as can be found here: https://api2.connexcs.com/

```
api.lookup.asrplus(number);
api.lookup.tps(number);
api.lookup.nnpc(number);
api.lookup.hlr(number);
api.lookup.lrn(number);
api.lookup.ping(number);
api.lookup.userspaceLookup(number);
```
### Send
A way to send messages from the system

```
api.send.email(params);
api.send.sms(params);
api.send.notify(params);
api.send.sip(params);
```

## Examples

### Random CLI

```
function main(){
  var data = vars();
  var cliList = ["11111","22222","33333","44444"];
  data.routing.cli = cliList[Math.floor(Math.random() * cliList.length)];
  return exit(data);
}
```
 
### Checking a DNC List

You can upload your own number lists for Do-not-Call, or white lists in the User Space Database inside the main system. You can then query it from Script Forge.

```
function main(){
	var data = vars();
	if (!data.routing) data.routing = {};
	api.lookup.userspaceLookup('DNC', data.routing.dest_number).then(function(rData){
		err([603, "Denied (DNC)"]);
	}).catch(function(){
		exit(data.routing);
	});
}
```

### Performing an ASRPlus Lookup

ASRPlus is a ConnexCS feature for reducing unecessary attempts and providing faster fails on calls. Its most suitable for agress call-center traffic profiles.

```
function main(){
	var data = vars();
	if (!data.routing) data.routing = {};
	api.lookup.asrplus(data.routing.dest_number).then(function(asrplusData){
		if (asrplusData.status && asrplusData.status =='Invalid'){
			err([604, "Number not Found (ASRPlus)"]);
		} else {
		  	data.routing.asrPlus = asrplusData;
			exit(data.routing);
		}
	});
}
```
