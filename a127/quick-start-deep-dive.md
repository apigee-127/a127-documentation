## About the example

Apigee 127 projects all follow a pattern similar to this hello world application. The hello world example lays down a project structure that includes a Swagger API specification (in YAML format), an implemented controller, and a base Node.js app. Let's look at these pieces one by one:

### Hello world project structure

The project files are cloned directly from the `https://github.com/apigee-127/project-skeleton` repository on GitHub. This skeleton project contains all the files and internal wirings needed for a basic Apigee 127 project. 


    hello-world-api
        /api
            /controllers
                hello_world.js
            /swagger
                swagger.yaml
        /config
            index.js
            /profiles
                default.json
                README.md
        app.js
        package.json
        README.md
     


### The Swagger compliant API model

You will find the API model in `api/swagger/swagger.yaml`. This file conforms to the Swagger 2.0 specification, which models API behavior in a standard way, and enables tooling that lets you model, test, and document your API on the fly. 

**Note:** YAML is a data serialization/representation standard. If you're new to YAML, check out [www.yaml.org](http://www.yaml.org). Another excellent introduction is the [Wikipedia YAML entry](http://en.wikipedia.org/wiki/YAML).

From the `hello-world` folder, execute:

    `a127 project edit`

This command launches the interactive Apigee Swagger editor, which loads the project's `swagger.yaml` file. 

![alt text](https://raw.githubusercontent.com/WWitman/docs/master/a127/images/swagger-editor.png)

Here is the entire hello-world app Swagger model. It conforms to the Swagger 2.0 specification, which is documented [here](https://github.com/reverb/swagger-spec/blob/master/versions/2.0.md). You'll notice there are several Apigee 127 specific extensions (`x-swagger-router-controller`, etc.) included in the model. These extensions are explained below. 

    swagger: 2
    info:
      version: 0.0.1
      title: Skeleton App
    host: localhost
    basePath: /
    schemes:
      - http
      - https
    consumes:
      - application/json
    produces:
      - application/json
    x-volos-resources: {}
    paths:
      /hello:
        x-swagger-router-controller: hello_world
        x-volos-authorizations: {}
        x-volos-apply: {}
        get:
          description: "Returns 'Hello' to the caller"
          operationId: hello
          produces:
            - application/json
          parameters:
            - name: name
              in: query
              description: The name of the person to whom to say hello
              required: false
              type: string
          responses:
            "200":
              description: Success
              schema:
                $ref: "#/definitions/HelloWorldResponse"
            default:
              description: Error
              schema:
                $ref: "#/definitions/ErrorResponse"
    definitions:
      HelloWorldResponse:
        required:
          - message
        properties:
          message:
            type: string
      ErrorResponse:
        required:
          - message
        properties:
          message:
            type: string


#### The 'paths' object and Swagger extensions

The `paths:`  object specifies the API's resource paths, HTTP verb, query parameters, and more. 

The `x-swagger-router-controller` extension maps a path to a controller file, where the HTTP operation's logic is implemented. For example, in the hello world app, the path `/hello` maps to a controller file `controller/hello_world.js`:

    
    paths:
      /hello:
        x-swagger-router-controller: hello_world
        x-volos-authorizations: {}
        x-volos-apply: {}
        get:
          description: "Returns 'Hello' to the caller"
          operationId: hello
    

**Note:** The extension `x-swagger-router-controller` is a custom Apigee 127 extension to the Swagger model. 

### Controller files

When a request is received, Apigee 127 looks at the endpoint (`/hello`) and routes the request to a controller file. In this example, the `controller/hello_world.js` implementation retrieves a query parameter sends back a "hello world" response to the caller:

    
    function hello(req, res) {
      var name = req.swagger.params.name.value;
      var hello = name ? util.format('Hello, %s', name) : 'Hello, stranger!';
      res.json(hello);
    }
  

### The main Node.js app and Apigee 127 middleware

The main `app.js` is a simple Express app consisting of these six lines:

    'use strict';
    var a127 = require('a127-magic');
    var express = require('express');
    var app = express();
    app.use(a127.middleware());
    app.listen(process.env.PORT || 10010);

The `a127-magic` module loads several Apigee 127 middleware modules. These modules perform tasks like Swagger specification validation, endpoint routing, and volos configuration:

   * a127-magic
      * swagger-tools
         * middleware
            * swagger-metadata.js
            * swagger-router.js
            * swagger-validator.js
      * volos-swagger
         * lib
            * connect-middleware.js
      * js-yaml
      * underscore

Let's look at the middleware components:

* [swagger-tools](https://www.npmjs.org/package/swagger-tools) 
Provides middleware functions for:
      * swagger-metadata -- Provides Swagger configuration metadata to downstream middleware components. 
      * swagger-router -- Uses Swagger middleware metadata to route requests to controllers. 
      * swagger-validator - Uses Swagger information to validate API requests before they are routed to controllers. 

* [volos-swagger](https://github.com/apigee-127/volos/tree/master/swagger)
Lets you configure volos features like caching and OAuth directly in the Swagger model file. For example, this stanza configures the volos "in memory" caching feature for your API. 

    x-volos-resources:
      cache:
      provider: "volos-cache-memory"
      options:
          - "memCache"
          - ttl: 10000

Similarly, you could configure caching using Redis or Apigee. Refer to the [volos-swagger](https://github.com/apigee-127/volos/tree/master/swagger) documentation for more information. 

The pattern:

    var a127 = require('a127-magic');
    app.express=require('express');
    var app = express();
    app.use(a127.middleware());

is all you need to add this support to your application. Behind the scenes, the "wiring" and interaction is driven through the Swagger configuration file. The basic programming model is:

1. Model your API using the Apigee Swagger editor. Define the API endpoints, request verbs, and optionally add features like volos-based caching, quota management, and OAuth. 
2. Implement controllers to handle each request endpoint. 
3. Drive, test, and document your API with the Swagger editor. 