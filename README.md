### Smartcar SDK

Node.js client SDK for the Smartcar API.

###Example Usage
```javascript
var Smartcar = require('node-sdk');
var express = require('express');

var app = expres();
var client = new Smartcar({
    clientId: 'your app id',
    clientSecret: 'your app secret',
    redirectUri: 'your callback',
    scope: [ 'read_vehicle_info' ]
});

app.get('callback endpoint', client.handleAuthCode, 
	function(req, res, next){
        res.redirect('main app endpoint');
	}
);
app.get('main app endpoint', function(req, res, next){
    var access = 'access token'
    client.refreshAccess(access)
    .then(function(newAccess){
        return client.getVehicles(newAccess.access_token);
    })
	.then(function(vehicles){
		return vehicles[0].info();
	}).then(function(info){
        do_something_with(info);
	});
});
app.get('homepage endpoint', function(req, res, next){
    res.redirect(client.getAuthUrl('oem name'));
});
app.listen(4000);
```

###TODO

 * handle refresh tokens
 * allow callbacks and promise style with nodeify
