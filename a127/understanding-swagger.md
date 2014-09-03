* What is Swagger?
* How does Apigee 127 use Swagger?
* Help me with YAML
* Understanding the Swagger specification file


## What is Swagger?

[Swagger™ ](https://helloreverb.com/developers/swagger) is a specification and framework implementation for describing, producing, consuming, and visualizing RESTful web services. 

To read more about Swagger, refer to:

* [The Swagger website](https://helloreverb.com/developers/swagger) 
* [Swagger on GitHub](https://github.com/wordnik)


## How does Apigee 127 use Swagger?

The Swagger Editor for Apigee 127 lets you interactively model, test, and document your Apigee 127 API. This editor is part of Apigee 127, and is installed when you install Apigee 127.

Use this editor to configure the `swagger.yaml` configuration file. A basic version of this file is provisioned with every new Apigee 127 project, and lives in `<project_root>/api/swagger/swagger.yaml`. Apigee 127 Swagger middleware extensions endpoint routing to controller files and the inclusion of features like caching, OAuth 2.0 security, quotas, and more. 

The `swagger.yaml` file conforms to the [Swagger 2.0 specification](https://github.com/reverb/swagger-spec/blob/master/versions/2.0.md). 

Behind the scenes, Apigee 127 Swagger middleware validates and processes the Swagger configuration file, and routes API operation endpoints to controller files. All **you** need to do is implement your custom API controller logic. 

**Try it:**

1. `a127 project create test-project`
2. `cd test-project`
2. `a127 project edit`

![alt text](https://raw.githubusercontent.com/WWitman/docs/master/a127/images/swagger-editor.png)

## Help me with YAML

YAML is a data serialization/representation standard. If you're new to YAML, check out [www.yaml.org](http://www.yaml.org). Another excellent introduction is the [Wikipedia YAML entry](http://en.wikipedia.org/wiki/YAML).

YAML is intended to be easy for humans to read. Every Apigee 127 project includes a Swagger 2.0 compliant configuration file that is written in YAML. 

## Understanding the Swagger specification file

When you execute `a127 project create myproject`, a default Swagger model is placed in `myproject/api/swagger/swagger.yaml`. This model conforms to the [Swagger 2.0 specification](https://github.com/reverb/swagger-spec/blob/master/versions/2.0.md). 

Here is the entire `swagger.yaml` file that is provisioned for a new Apigee 127 project: 

```yaml
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
```
The Swagger file includes a number of standard Swagger 2.0 specification elements. You can read about them in the [Swagger 2.0 specification](https://github.com/reverb/swagger-spec/blob/master/versions/2.0.md). 

Here's a brief description of the elements in the Apigee 127 Swagger file:

*  **swagger: 2** - (Required) Identifies the version of the Swagger specification (2.0).

*  **info:** - (Required) Provides metadata about the API.

*  **host:** - The host serving the API. By default, a new project connects to a server running locally on port 10010. 

*  **basePath:** - The base path on which the API is served, which is relative to the host. 

*  **schemes:** - A list of transfer protocol(s) of the API.

*  **consumes:** - A list of MIME types the APIs can consume.

*  **produces:** - A list of MIME types the APIs can produce.

*  **x-volos-resources:** - A custom Swagger extension for [volos-swagger](https://github.com/apigee-127/volos/tree/master/swagger). The volos-swagger module lets you add  `volos` features like quotas, caching, and OAuth 2.0 to your API by configuring in the Swagger file. For more information, see the [volos-swagger README file](https://github.com/apigee-127/volos/tree/master/swagger).

*  **paths:** - (Required) Defines the available operations on the API. You'll spend most of your time configuring the paths part of the file. You can read about the path element in the [Swagger 2.0 specification](https://github.com/reverb/swagger-spec/blob/master/versions/2.0.md). In general, the paths section specifies an operation's verb (like `get`), the endpoint for an API operation (like `/hello`), query parameters, and responses. 

In the Apigee 127 Swagger file, the paths section also includes these custom extensions:

* **x-swagger-router-controller:** - This extension specifies the name of the controller file (hello_world.js) that will execute when this API operation is called. Controller files reside in `apis/controllers` in your Apigee 127 project. This extension is provided through the `swagger-tools` middleware module, which is included when you require the `a127-magic` module in your main Node.js app.

* **x-volos-authorizations:** - This extension applies volos authorization to the API operation. You can use this extension to add OAuth 2.0 security, for example. For more information, see the [volos-swagger README file](https://github.com/apigee-127/volos/tree/master/swagger). ??? IS THIS APPLYING A DEFAULT? OR IS IT SIMPLY UNDEFINED? 

* **x-volos-apply:** - This extension applies specified volos modules, and they use the configurations specified in the x-volos-resources extension, described previously. For more information, see the [volos-swagger README file](https://github.com/apigee-127/volos/tree/master/swagger)

* **definitions:** - ??? THIS ISN'T COVERED IN THE SWAGGER 2.0 SPEC-- WHAT IS THIS SECTION FOR???