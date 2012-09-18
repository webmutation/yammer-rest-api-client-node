yammer-rest-api-client
======================
This is a client for [Yammer's RESTful API](https://developer.yammer.com/api) for Node.js. The module only implements some of the basic components of the API, but it can be easily extended to support additional functionality.

Currently, the following are supported:

* Messages (viewing): 100 %
* Messages (manipulating): 100 %
* Messages (email): 0 %
* Users: 100 % 
* Groups: 100 %
* Topics: 100 %
* Likes: 0 %
* Group memberships: 0 %
* Relationships: 0 %
* Suggestion: 0 %
* Threads: 0 %
* Subscriptions: 0 %
* Autocomplete: 0 %
* Invitations: 0 %
* Search: 0 %
* Networks: 100 % (implemented as part of the Users API module with YammerAPIClient.users.networks)
* Access tokens: 0 %
* Pending attachments: 0 %

Please note that you must be the admin of the network in order to be able to use some of the functionality, e.g. create new users.

Using the client
================

The main object containing the API methods is YammerAPIClient, that requires a valid OAuth 2.0 access token to perform the API calls. The library will *not* generate these tokens for you so applications must have some other way to generate the tokens.

Once a token is available, create a new API client and call the needed method in the _messages_, _users_, _groups_, or _topics_ namespaces. The sample code below performs a call to retrieve messages from the "All Company" stream, limiting to 10 per page and in reverse order:

```
var YammerAPIClient = require('yammer-rest-api-client-node'),
	client = new YammerAPIClient({ token: "your-oauth-2.0-token" });

client.messages.all({limit: 10, reverse: true }, function(error, data) {
	if(error)
		console.log("There was an error retrieving the data");
	else {
		console.log("** Data was retrieved **");
		console.log(data);
	}
})
```

The module is available from NPM:

```
"dependencies": {
    "yammer-rest-api-client-node": ">= 0.0.0"
}
```

Then, run ```npm install``` to force npm to retrieve the needed dependencies.

Methods, parameters and callbacks
=================================

The following methods are supported:

###Messages:
* messages.all(params, callback)
* messages.sent(params, callback)
* messages.received(params, callback)
* messages.private(params, callback)
* messages.following(params, callback)
* messages.in_thread(id, params, callback)
* messages.from_user(id, params, callback)
* messages.about_topic(id, params, callback)
* messages.in_group(id, params, callback)
* messages.liked_by(id, params, callback)
* messages.delete(id, callback)
* messages.create(data, callback)
* messages.get(id, callback)

###Users:
* users.list(params, callback)
* users.get(id, callback)
* users.create(data, callback)
* users.delete(id, callback)
* users.update(id, params, callback)
* users.networks(callback)
* users.current(callback)

###Topics:
* topics.get(params, callback)

###Groups:
* groups.list(params, callback)
* groups.get(id, callback)
* groups.create(data, callback)
* groups.update(id, data, callback)

Methods that support additional URL (GET style) parameters such as [messages.all or messages.received](https://developer.yammer.com/api/#message-viewing), they are provided in the params structure using the same field names as described in the official Yammer API. 

Methos that require JSON structures such as [user.create](https://developer.yammer.com/api/#users) will be provided such structures also in the position of the params parameter. Since the structures vary depending on the API method please consult the documentation for each, as the client does not perform any kind of parameter and value checks before sending the data and will not warn you if the structure is incorrect.

In some cases an additional id parameter is required (users.get, messages.get, etc); when that's the case, the value must be provided as a standalone parameter in the first parameter position when calling the function.

Callbacks are defined as function(error, data). If there was an error during the process, an HttpResponse object will be returned as the error parameter. For successful requests, error will be "undefined" and data will contain the response as a native object.

Testing
=======
Unit tests are implemented with [Vows](http://vowsjs.org) and are located in the tests/ folder. Not all methods are currently tested.

In order to execute the unit tests, edit first file tests/testconfig.js and provide your own OAuth 2.0 access token.

License
========
Apache Software License 2.0: http://www.apache.org/licenses/LICENSE-2.0

Changelog
=========
**Version 0.1.0**: Initial version

TODO
====
* Implement the remaining API methods
* Increase unit testing coverage