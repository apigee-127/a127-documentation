# Apigee 127 API BaaS example

This example shows how create an API that accesses data from a BaaS solution using Apigee 127. The BaaS in this example is Apigee's API BaaS, running on Apigee Edge. 

# What the example demonstrates

This example shows how to create an Apigee 127 API project that accesses an API BaaS data store. We'll demonstrate how to:

* Set up the API BaaS data store
* Create a new Apigee 127 project
* Use the Swagger editor to model the API and wire up routes
* Implement API logic using controllers
* Test the API

# Getting started

If you'd like to create this project and run it, you'll need to do some preliminary setup:

1. Install Apigee 127. See (xref to the installation doc).
2. Create an Apigee API BaaS account, if you don't already have one. See (xref to that).
3. Create a new directory for your Apigee 127 project.

# Setting up the API BaaS data store

If you're new to API BaaS, we'll walk through the steps for setting up and populating a simple data store. 

If you're familiar with API BaaS, here's what you need:

* Create a new App in your organization called `petstore`.
    - Select ADD NEW APP and fill out the dialog.

* Create a new data collection
    - Select DATA in the left column. 
    - Click the New Collection icon. 
    - Create a collection called pets.

* Add data to the pets collection. 
    - Click CREATE.
    - Enter data in the JSON Body field, following a pattern like this:

   {
       "name" : "value",
       "id" : "value",
       "breed" : "value",
       "age" : "value"
    }

For example:

    {
       "name" : "Lucy",
       "id" : "0001",
       "breed" : "Lab",
       "age" : "2"
    }

Click Run Query to enter the data. Add more if you like. That's it. 

# Create an Apigee 127 project

Go to your project's root directory and execute:

1.     `a127 create project petstore-project`
2.     `npm install`

These commands create a skeleton Apigee 127 project for you and populate Node.js dependencies. 

# Model the API

Start the Swagger editor:

`a127 project edit`

Make the following changes. You're modelling the new API with a /petstore resource that returns a list of pets. Through the Swagger definition, /petstore maps to a method getPets() in the controller file `petstore.js`. 

1. "title": "A127 API BaaS example"
2.  "paths": {
    "/petstore": {
      "x-swagger-router-controller": "petstore",
3. "get": {
        "description": "Returns data from the API BaaS data store",
        "operationId": "getPets"

4. "responses": {
          "200": {
            "description": "Success",
            "schema": {
              "$ref": "#/definitions/PetStoreResponse"

5.  "definitions": {
    "PetStoreResponse": {
      "required": [ "message" ],
      "properties": {
        "message": {
          "type": "string"

````
{
  "swagger": 2.0,
  "info": {
    "version": "0.0.1",
    "title": "A127 API BaaS example"
  },
  "host": "localhost",
  "basePath": "/",
  "schemes": [
    "http",
    "https"
  ],
  "consumes": [
    "application/json"
  ],
  "produces": [
    "application/json"
  ],
  "x-volos-resources": {
  },
  "paths": {
    "/petstore": {
      "x-swagger-router-controller": "petstore",
      "x-volos-authorizations": {
      },
      "x-volos-apply": {
      },
      "get": {
        "description": "Returns data from the API BaaS data store",
        "operationId": "getPets",
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "name",
            "in": "query",
            "description": "The name of the person to whom to say hello",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "Success",
            "schema": {
              "$ref": "#/definitions/PetStoreResponse"
            }
          },
          "default": {
            "description": "Error",
            "schema": {
              "$ref": "#/definitions/ErrorResponse"
            }
          }
        }
      }
    }
  },
  "definitions": {
    "PetStoreResponse": {
      "required": [ "message" ],
      "properties": {
        "message": {
          "type": "string"
        }
      }
    },
    "ErrorResponse": {
      "required": [ "message" ],
      "properties": {
        "message": {
          "type": "string"
        }
      }
    }
  }
}
````


# Implement the configuration

````
var ug = require('usergrid');

exports.usergrid = {
  organization: 'wwitman',
  application: 'petstore',
  authType:ug.AUTH_CLIENT_ID,
  clientId: 'YXA65gOHUBylEeSjJ-HQxxTNyA',
  clientSecret: 'YXA6-kHeqrwwkHedlqsBDZCz-ae7q00',
  logging: false
};
````

# Implement the controller

1. Rename controllers/hello_world.js to petstore.js. 
2. Implement the "get" method:

````
'use strict';

var petstore = require('../helpers/petstore');

module.exports = {
  get: get
}

function get(req, res, next) {
        
    petstore.get(function(err, reply) {
        if (err) { return next(err); }
        res.json(reply);
      });       
}
````

3. Implement the helper file:

````
'use strict';

var express = require('express');
var config = require('../../config').usergrid;
var usergrid = require('usergrid');

exports.get = get;

var app = express();

var client = new usergrid.client({
    
      orgName: config.organization,
      appName: config.application,
      authType:usergrid.AUTH_CLIENT_ID,
      clientId: config.clientId,
      clientSecret: config.clientSecret,
      logging: false
    
});

function get(cb) {
    var options = {
        method:'GET',
        endpoint:'pets'
    };
    client.request(options, function (err, data) {
        
        if (err) {
            console.log('GET failed');
        } else {
            //data will contain raw results from API call
            var result = JSON.stringify(data);
            
            cb(err, result);
            
        }
    });
}
````

````

# Configure the project

<< Add the config file information here >>

# Test the project

<< curl commands here >>



## Quick start and basic work flow

Creating and deploying a new Apigee 127 project is quick and easy. 

1. Create a new Apigee-127 project:

    `a127 project create <project-name>`

    This command clones a skeleton Apigee-127 project from GitHub. The skeleton includes the base Node.js app with pre-built wirings for your project. It includes a "hello world" starter project (see xref to hello world readme). 

2. (Optional) Open the Swagger editor.  

    `a127 project edit`
    
    This command launches the Swagger editor, which you can use to define, validate, and test your API model. API operations are  automatically stubbed out in Node.js in the `/controllers` directory. The editor also creates documentation for your API on the fly as you develop. 

3. (Optional) Download and start Usergrid. 

    `a127 usergrid start`
    
    This step is optional, however, Usergrid provides a locally installed BaaS platform where you can create data sets for testing your API. If you're running command for the first time, Usergrid will be downloaded to your local machine first. 

4. (Optional) Start the Usergrid portal. 

    `a127 usergrid portal`
    
    The portal provides a UI for creating and managing data sets. You can interact with Usergrid data with the usergrid npm module.

5. (Optional) Create a deployment provider account. 

    `a127 account create <account-name>`
    
    A provider is a cloud-based platform where you can deploy your API. Currently, the only supported provider is Apigee Edge. Others will be added soon. 

6. (Optional) Deploy your project to a deployment provider. 

    `a127 project deploy`
    
    Deploys your project to a provider account. For example, you can deploy your project to Apigee Edge, and take advantage of Edge's enterprise-class API features like caching, rate-limiting, security, resource-based access control, and analytics.