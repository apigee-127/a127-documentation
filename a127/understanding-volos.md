# What is Volos.js?

Volos.js is a set of software modules that make it possible for developers to add common API design patterns  like security and traffic management to their code. Using Volos.js, you can develop and test your API locally, and then deploy to the cloud for greater scalability and performance.

#Example

For example, here's how you caching to your API using Volos.js. 

First, instantiate a Volos.js `cache` object. It takes a configuration object and sets a "time to live" value of 1 second for the cache. Use this pattern to add caching to your API:

    // create Volos cache
    var cache = volos.cache.create('cache', _.extend({
      ttl: 1000
    }, volos.config));

Next, create a middleware object that can be used with a Connect, Express, or Argo proxy server. In this case, a Connect server is the middleware target:

    var cacheMiddleware = cache.connectMiddleware().cache();

Finally, you need a server:

    proxy.createProxy(cacheMiddleware);

In this case, the createProxy() method creates a [Connect](https://www.npmjs.org/package/connect) HTTP server. For the complete implementation of this example, see the [training sample](https://github.com/apigee-127/volos/blob/master/samples/training/README.md). 

# Volos.js features

Volos.js includes Node.js modules for adding these features to an API:

* **Caching:** Response caching that can be configured by URI or custom function.

* **Analytics:** Analytics that can published to [Apigee Edge Analytics](http://apigee.com/about/resources/videos/edge-analytics-dashboard-tour).

* **OAuth 2.0:** Full OAuth 2.0 Server or OAuth 2.0 proxy to Apigee Edge.

* **Quota:** Quota on a per-API, per-resource, per-header or per-parameter basis, or with a customized function.

# Execution environment options

Developers can choose to configure Volos.js to run in several different modes:

* **[Deployed on Apigee Edge](#api-deployed-on-apigee-edge):** When Volos applications are deployed to the Apigee platform, they will take advantage of local optimizations so that they run with the best possible availability and performance. They can also add optimizations such as detailed API analytics.

* **[Deployed anywhere](#api-running-in-conjunction-with-apigee-edge):** Volos.js can run in any environment that supports Node.js and communicate with Apigee Edge using APIs, so that the full power of Apigee can be used to manage and configure developers, apps, and APIs.

* **[Standalone mode](#running-the-api-independently-of-apigee-edge):** Volos.js supports a standalone mode that relies entirely on open-source components.

**Note:** If you run Volos.js on Apigee Edge or in conjunction with Apigee Edge, you must have an Apigee account and deploy a special proxy to Edge. The deployment steps are simple, and are covered here. (TBD -- add link)

### <a herf='api-deployed-on-apigee-edge'></a>API deployed on Apigee Edge

You can deploy and run the entire API implementation on the Apigee Edge platform. 

![Alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/on-edge.png)

1. An app makes an API call to Apigee Edge. 
2. Volos leverages direct access to Apigee Edge Services.
3. The API call is processed using the normal Apigee Edge flow. 

## <a href='api-running-in-conjunction-with-apigee-edge'></a>Deployed anywhere and running in conjunction with Apigee Edge

Running locally or deployed to any Node.js environment of your choice, your API make calls to Apigee Edge to handle activities like caching, quota management, and OAuth. Volos provides the glue that binds together your API implementation and Apigee Edge. 

![Alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/with-edge.png)

1. The app makes an API call and the endpoint is the Apigee agent (Volos.js). 
2. Volos.js calls Apigee Edge to perform OAuth, Quota, or Caching. Volos.js only sends metadata to Edge, not the API payload. 
3. Edge returns a response indicating whether to allow the API call.
4. If allowed, the API call is proxied to the Node.js API implementation.
5. The API response is returned from the API implementation.
6. (Optional) Metadata is sent to Edge for centralized analytics and monitoring. 

## <a href='running-the-api-independently-of-apigee-edge'></a>Standalone: Running the API independently of Apigee Edge

You can also run your API locally, with caching and quota information either stored in-memory or in a Redis key/value store. A simple configuration lets you chose between the two. 

# Further Reading

* Volos.js on [GitHub](https://github.com/apigee-127/volos)

* Getting started [training sample](https://github.com/apigee-127/volos/blob/master/samples/training/README.md)