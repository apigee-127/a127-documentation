# What is Apigee 127?

* Overview of Apigee 127

* Apigee 127 programming model
    * API running in conjunction with Apigee Edge
    * API running on Apigee Edge
    * API running independently of Apigee Edge

## Overview of Apigee 127

Apigee 127 provides the tools you need to design and build Enterprise-class APIs entirely in Node.js. 

### Apigee 127 is simple to understand

1. Start by modeling your API with the Swagger editor. Try it out. Change it around. Kick the tires. 
2. Add Enterprise-class features like caching, quotas, and OAuth security to your Swagger model with easy-to-configure Swagger extensions. 
3. Seamlessly move your API to Node.js where your API takes advantage of Apigee 127 middleware, providing the "glue" needed for caching, quota support, OAuth 2.0 security, and other Enterprise-class features.
4. Implement custom controllers for each of you API endpoints in Node.js. 
5. Run and test your API locally, or deploy it to a platform like Apigee Edge, Heroku, or Elastic Beanstalk. .

### A little more detail...

* Create and run a new API project in seconds with the "a127" command-line interface. It's as simple as `a127 project create myproject`. 

* Design, test, and document your API with the Apigee 127 Swagger editor. To start the editor, do `a127 project edit`.

  The highly interactive and intuitive Swagger editor lets you define and refine your API's operations rapidly, test them, and document them within the context of an Apigee 127 project. The API model is automatically saved in your project, and provides the "wiring" to call your custom API endpoint controllers. 

    ![alt text](https://raw.githubusercontent.com/WWitman/docs/master/a127/images/swagger-editor.png)

* Integrate Enterprise-class API features like caching, quotas, OAuth, and more into your API either in Node.js code or entirely through configuration in the Swagger editor. These features rely on the `volos` module. 

    Integrate caching in the Swagger configuration with a custom Volos.js extension:

        x-volos-resources:
            cache:
              provider: "volos-cache-memory"
              options:
                - "memCache"
                -
                  ttl: 10000

    Or implement the same feature in Node.js code (through the Volos.js module):

        var cm = require('volos-cache-memory');
        var cache = cm.create('name', { ttl: 10000 }); 
        cache.set('key', 'value');
        cache.get('key', callback);

* Implement custom controllers in Node.js to handle the API endpoints (like `/hello` for example!) defined in the Swagger model. 

        function hello(req, res) {
          var name = req.swagger.params.name.value;
          var hello = name ? util.format('Hello, %s', name) : 'Hello, stranger!';
          res.json(hello);
        }
  

### The middleware is the key

Swagger Tools middleware provides Swagger specification validation, wires up routes defined in the Swagger model to controllers, and generally, provides the "glue" that binds the Swagger model to your implementation.

The `a127-magic` module loads a module called `volos-swagger`. This module allows you to add [Volos.js](https://github.com/apigee-127/volos) functionality like caching, quota, and OAuth entirely through configuration. 

For example, this pattern:

        var a127 = require('a127-magic');
        app.express=require('express');
        var app = express();
        app.use(a127.middleware());

is all you need to add this support to your application. 


## The Apigee 127 programming model

The programming flow for an Apigee 127 project looks like this:

![Alt text](https://raw.githubusercontent.com/WWitman/docs/master/a127/images/programming-model.png)

* Create an Apigee 127 project using the command-line interface. It's as simple as `a127 project create myproject`.

* Use the Swagger editor to model, test, and document your API model. 

* Apigee 127 Node.js middleware modules validate and process the resulting Swagger definition. 

* Behind the scenes, Apigee 127 wires up your app, routing HTTP requests to specific Node.js controller files. 

* You implement your custom API logic in the controller files. 

* Using `volos` modules, you can add caching, quota, and OAuth security to your API. You can integrate `volos` features directly into your Swagger model or implement them programmatically in the Node.js app. 

* You can run your app on Apigee Edge or in conjunction with Apigee Edge. Or, you can run your app locally or deploy it to another platform, like Heroku or Elastic Beanstalk.


### API running in conjunction with Apigee Edge

Running locally, your API make calls to Apigee Edge to handle activities like caching, quota management, and OAuth. Volos provides the glue that binds together your API implementation and Apigee Edge. 

![Alt text](https://raw.githubusercontent.com/WWitman/docs/master/a127/images/with-edge.png)

1. The app makes an API call and the endpoint is the Apigee agent (Volos). 
2. Volos calls Apigee Edge to perform OAuth, Quota, or Caching. Volos only sends metadata to Edge, not the API payload. 
3. Edge returns a response indicating whether to allow the API call.
4. If allowed, the API call is proxied to the Node.js API implementation.
5. The API response is returned from the API implementation.
6. (Optional) Metadata is sent to Edge for centralized analytics and monitoring. 


### Running the API on Apigee Edge

You can deploy and run the entire API implementation on the Apigee Edge platform. 

![Alt text](https://raw.githubusercontent.com/WWitman/docs/master/a127/images/on-edge.png)

1. An app makes an API call to Apigee Edge. 
2. Volos leverages direct access to Apigee Edge Services.
3. The API call is processed using the normal Apigee Edge flow. 

### Running the API independently of Apigee Edge

TBD. 