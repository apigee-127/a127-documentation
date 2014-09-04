# What is Apigee 127?

* [Overview ](#overview)
* [Programming Model](#programming_model)

## <a name="overview"></a> Overview of Apigee 127

Apigee 127 provides the tools you need to design and build Enterprise-class APIs entirely in Node.js and deploy them on any Node.js runtime.

Here's what we include:
* A [Swagger 2.0 Editor] [swagger-editor] running locally, built for the Swagger Community by Apigee 
* [Swagger Tools] [swagger-tools-github] Middleware for Node.js including Message Validation, Authorization and Routing
* [Volos.js] [volos-github] Middleware for value-added functions such as Caching, Quota and OAuth 2.0
* A Node.js version of [apigeetool] [apigeetool-github] for deploying your application to Apigee Edge
* [Apigee-127 Command Line Interface] [a127-cli] for managing project lifecycle
* An easy-to-manage local [Usergrid] [usergrid] runtime and portal

## <a name="programming_model"></a>The Model-First Apigee 127 Programming Model

The focal point of the Apigee-127 experience is using a standard model for building APIs.  From this standard model a great deal can be inferred from your API, such as:
* What type of resources are needed to make it run
* What authorization scopes should be applied to which paths?

Combining the `swagger-tools` and Volos.js middleware Apigee-127 enables you to quickly build robust, high quality APIs by offloading a lot of the work needed to do so.  From the Swagger 2.0 model we can create the server-side flows and middleware specified in the specification and leave the core logic of the API up to API developers.

The programming flow for an Apigee 127 project looks like this:

![Alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/programming-model.png)

* Define the Swagger Model using the Swagger 2.0 Editor included with Apigee-127.
* Annotate your resources and operations in the Swagger 2.0 model with the `x-swagger-router-controller` extension to define the name of the Controller that implements the logic behind the operation.  Example:
```yaml
paths:
  /hello:
    x-swagger-router-controller: "hello_world"  
```
* Utilize the `operationId` property for your operations in the Swagger 2.0 Model
```yaml
    get:
      description: "Returns 'Hello' to the caller"
      operationId: "hello"
```
* Optionally use the Volos.js Swagger Extensions to define Caching, Quota and OAuth configuration:
```yaml
x-volos-resources:
  cache:
    provider: volos-cache-memory
    options:
      name: name
      ttl: 10000
  quota:
    provider: volos-quota-memory
    options:
      timeUnit: minute
      interval: 1
      allow: 2
  oauth2:
    provider: volos-oauth-apigee
    options:
      key: *apigeeProxyKey
      uri: *apigeeProxyUri
      validGrantTypes:
        - client_credentials
        - authorization_code
        - implicit_grant
        - password
      passwordCheck: passwordCheck
```
* Optionally apply Volos.js functions to individual operations to add caching, quota and OAuth security to your API. You can integrate `volos` features directly into your Swagger model using the Volos.js Swagger Extensions or implement them programmatically in the Node.js app.  Example:
```yaml
  /twitter:
    x-swagger-router-controller: twitter
    x-volos-authorizations:
      oauth2: {}
    x-volos-apply:
      cache: {}
      quota: {}
    get:
       ...
```
* Behind the scenes, Apigee 127 wires up your app, routing HTTP requests to specific Node.js controller files. 
* At runtime the `swagger-router` will validate & authorize the request before sending it to the 'hello' operation of the 'hello_world' controller.  By default the swagger-router looks for controllers in `[project_home]/api/controllers`
* If configured, Volos.js Middleware will handle Caching, Quota and/or OAuth authorization
* Finally, your controller logic will be invoked according to the `x-swagger-router-controller` specified for the resource path and the `operationId` of the corresponding operation.  By default the Controller should be in `[project_home]/api/controllers/[x-swagger-router-controller].js`
* You can develop and test your API locally using the command `a127 project start`
* Once you are ready to deploy your API it can run anywhere Node.js can run: custom servers, Apigee Edge, as well as other PaaS providers like Heroku or Amazon Elastic Beanstalk.  
** However, to take advantage of the full suite of Apigee's value-added services the API should be deployed there. 

Want to get started?

* [Quick start](https://github.com/apigee-127/a127-documentation/wiki/Quick-start)
* [Quick start deep-dive](https://github.com/apigee-127/a127-documentation/wiki/Quick-start-deep-dive)

[volos-github]: https://github.com/apigee-127/volos
[a127-cli]: https://github.com/apigee-127/a127
[swagger-editor]: https://github.com/wordnik/swagger-editor
[swagger-tools-github]: https://github.com/apigee-127/swagger-tools
[apigeetool-github]: https://github.com/apigee/apigeetool-node/tree/master
[usergrid]: http://usergrid.incubator.apache.org/