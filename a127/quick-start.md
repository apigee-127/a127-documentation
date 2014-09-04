# Quick start

The purpose of this Quick Start is to show you how easy and quickly you can get a simple API up and running using Apigee-127.

## First steps
1. `npm install -g apigee-127` (as described in [Installation](https://github.com/apigee-127/a127-documentation/wiki/Installation)). 
3. Create a root folder for your Apigee 127 projects and cd to that folder.
4. Execute: `a127 project create hello-world`
5. `cd hello-world`
6. `npm install`
7. `a127 project start`
9. In another terminal, run: `curl http://localhost:10010/hello?name=Me`.  You should see the response `Hello, Me`.

That's it - You have now created and started your first project with Apigee-127!  

To understand what happens behind the scenes see: [Quick Start Deep Dive] (https://github.com/apigee-127/a127-documentation/wiki/Quick-start-deep-dive) 

## Next steps
* [Add a Quota] [quickstart-add-qouta]
* [Add a Path] [quickstart-add-path]
* [Operating in Mock Mode] [quickstart-mock-mode]
* [Implement a Controller] [quickstart-add-path]
* [Add Caching] [quickstart-add-caching]
* [Add Apigee OAuth] [quickstart-add-oauth-apigee]
* [Add Apigee Analytics] [quickstart-add-analytics-apigee]
* [Deploy to Apigee Edge] [quickstart-deploy-edge]


Note: The [Skeleton Project] [skeleton] may change over time and the documentation here may not keep up.  If you have ideas to improve the skeleton - you know the drill: [Fork] [fork] & [Pull] [pull]


[skeleton]: https://github.com/apigee-127/project-skeleton
[fork]: https://help.github.com/articles/fork-a-repo
[pull]: https://help.github.com/articles/using-pull-requests
[quickstart-add-qouta]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-Quota
[quickstart-add-path]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-a-New-Path
[quickstart-mock-mode]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Operating-in-Mock-Mode
[quickstart-add-caching]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-Caching
[quickstart-deploy-edge]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Deploy-To-Apigee-Edge
[quickstart-deploy-heroku]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Deploy-To-Heroku
[quickstart-deploy-beanstalk]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Deploy-To-EB
[quickstart-add-oauth-apigee]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-Apigee-OAuth
[quickstart-add-analytics-apigee]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-Apigee-Analytics