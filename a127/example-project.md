# Apigee 127 Example Project

This example gets you up and running quickly with a sample Apigee 127 project. 

* Installation
* Setup steps
* Configure and run the app
* About the app
* Troubleshooting

## Installation

1. Clone the example project from GitHub:

    `https://github.com/apigee-127/example-project.git`

2. In the `example-project` folder, execute: `npm install`.

3. Install Apigee 127:

    `npm install -g apigee-127`

    The `-g` option automatically places the `a127` executable commands in you PATH. If you do not use `-g`, then you need to add the `apigee-127/bin `directory to your PATH manually. You may need to use `sudo` to execute this command.

## Project setup

To run the example project, you need to set up these things first:

1. Create a [Twitter app](https://dev.twitter.com/apps). Any dummy app will do. You'll need to grab the secret keys and tokens from the app later. If you use keys from an existing app, be sure they are not expired. 
2. If you don't have one, create an [Apigee Edge account](https://enterprise.apigee.com).
3. Deploy the `apigee-remote-proxy` to Apigee Edge. Let's walk through the steps:
        
    3. Create an Apigee 127 account to hold your Apigee account information. Enter this command and follow the prompts:
    
        `a127 account create <anAccountName>`
        
        For example:
            
            $ a127 account create myApigeeAccount
            
            [?] Provider? apigee
            [?] Do you have an account? Yes
            [?] Organization? wwitman
            [?] User Id? wwitman@apigee.com
            [?] Password? *********
            [?] Environment? test

    4. Deploy the proxy:
    
        `a127 account deployApigeeProxy myaccount`
        
            Deploying proxy to wwitman
            Deployment
            name: apigee-remote-proxy
            environment: test
            revision: 3
            state: deployed
            basePath: /apigee-remote-proxy
            uris:
              - "http://wwitman-test.apigee.net/apigee-remote-proxy"
              - "https://wwitman-test.apigee.net/apigee-remote-proxy"

        **What is the apigee-remote-proxy?**

        This Apigee Edge proxy allows your local [`volos` components](https://github.com/apigee-127/volos) to talk to Apigee Edge through its management API. To be used by `volos`, the proxy must be deployed to Edge. The proxy is installed with Apigee 127, and lives in the `apigee-127/node_modules/Volos/proxy` directory. 

## Configure and run the app

1. In the `example-project`, copy `config/secrets.sample.js` to `config/secrets.js`.
2. Edit `config/secrets.js` with your Twitter keys and tokens. You can find them on the management console for your Twitter app, under the API Keys tab.
    
        exports.twitter = {
          consumer_key:        'xxxxxxxxxxxxxxxx',
          consumer_secret:     'yyyyyyyyyyyyyyyy',
          access_token:        'aaaaaaaaaaaaaaaa',
          access_token_secret: 'bbbbbbbbbbbbbbbb'
        };
    

3. In the same file, edit the Apigee Edge object with your Edge account information. For now, leave the `uri`, `key`, and `secret` attributes blank.

        exports.apigee = {
          organization: 'jdoe',
          uri: '',
          key: '',
          secret: '',
          user: 'jdoe@apigee.com',
          password: 'mypassword'
        };
4. (Optional) Edit the file `config/index.js` and change the default values for the devRequest and appRequest objects:  

    exports.devRequest = {
      firstName: 'Scott',
      lastName: 'Ganyo',
      email: 'sganyo@apigee.com',
      userName: 'sganyo@apigee.com'
    };

    exports.appRequest = {
      name: 'Test App'
    }; 

5. Execute: `node bin/create-app.js`. This uses the `volos-management-apigee` module to provision a user, a developer app, and a product for you on Apigee Edge. Later, you'll call the Twitter search API with an access token created on behalf of this developer. 

    You'll get a response like this:

        $ node bin/create-app.js 
           Creating developer <Name of developer in index.js>
           Creating application Test App for developer <Name of developer in index.js>
          { id: 'xxxx-yyyy-aaaa-bbbb-cccc',
          name: '<Name of App in index.js>',
          status: 'approved',
          developerId: 'xxxxxxxxxxx',
          callbackUrl: undefined,
          scopes: [ '' ],
          attributes: {},
          credentials: 
           [ { key: 'xxxxxxxxxxxxxxxxxxxxxx',
               secret: 'yyyyyyyyyyyyy',
               status: 'approved' } ] }
$ 


    **Note:** You can go to the Edge management UI to verify that the user, product, and developer app were all created. Look under the Publish menu.

6. Open `config/secrets` one more time. Edit the `uri`, `key`, and `secret` attributes as follows:

    1. For the `key` and `secret` values, add the `key` and `secret` values returned by the previous `bin/create-app.js` command. 
    
        **Note:** Be sure to copy the entire key and secret values. Sometimes if a key/secret contains a hyphen, double-clicking won't select the entire string.

    2. For the `uri`, enter `https://ORG.apigee.net/volos-proxy`. Substitute `ORG` with the name of your organization. For example:

        exports.apigee = {
          organization: 'jdoe',
          uri: 'https://jdoe-test.apigee.net/volos-proxy',
          key: 'xxxxxxxxxxxxxxxxxxxxxxxxx',
          secret: 'yyyyyyyyyyy',
          user: 'jdoe@apigee.com',
          password: 'mypassword'
        };  

    **Note:** These credentials allow the app to obtain an access token on behalf of the user through the Edge management OAuth 2.0 API. 
    
7. Execute: `a127 project start`.
8. Try the example curl commands that are printed to the console (from another console window):

    **Twitter Search:** This command calls the Twitter search API through the Apigee 127 proxy, running locally on your machine. The OAuth Bearer token was generated through Volos and added to this curl command by the app:

        ``curl -H "Authorization: Bearer aaaaaaaaaaaaaaaaaaaaa" "http://localhost:10010/twitter?search=apigee"``


    **Get a Password Token:** This command returns a new access token on behalf of the user. You can try substituting this access token in the Twitter search API call. 

        ``curl -X POST "http://localhost:10010/accesstoken" -d "grant_type=password&client_id=xxxxxxxxxxxxx&client_secret=yyyyyyyyyy&username=jdoe&password=password"``

## About the app

This example application implements an API proxy for the Twitter search API. A Node.js server running on your local system communicates with Apigee Edge through the volos Node.js module's management API. 

Volos is an open source Node.js solution for developing and deploying production-level APIs. Volos provides a way to leverage common features such as OAuth 2.0, Caching, and Quota Management into your APIs. Volos can proxy support for these features through Apigee Edge, or independently of Edge. 

This example runs locally and makes calls to Apigee Edge to handle OAuth requests. Volos provides the glue that binds together your API implementation and Apigee Edge. 

![Alt text](https://raw.githubusercontent.com/WWitman/docs/master/a127/images/with-edge.png)

1. The app makes an API call and the endpoint is the Apigee agent (Volos). 
2. Volos calls Apigee Edge to perform OAuth, Quota, or Caching. Volos only sends metadata to Edge, not the API payload. 
3. Edge returns a response indicating whether to allow the API call.
4. If allowed, the API call is proxied to the Node.js API implementation.
5. The API response is returned from the API implementation.
6. (Optional) Metadata is sent to Edge for centralized analytics and monitoring. 


## Troubleshooting

* A possible source of error is using incorrect or expired app keys in your configuration. Make sure, for example, that your Twitter credentials are current.