##Troubleshooting common problems

* A possible source of error is using incorrect or expired app keys in your configuration. Make sure, for example, that your Twitter credentials are current.

* If your app needs to parse a request body, be sure to place the bodyParser() before including the Apigee 127 middleware. Here is the recommended pattern:

        var a127 = require('a127-magic');
        var express = require('express');
        var app = express();
        app.use(express.bodyParser()); // Call this before including the a127.middleware()
        app.use(a127.middleware());
        app.listen(process.env.PORT || 10010);
   
    