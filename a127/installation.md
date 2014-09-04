## Installation

The `apigee-127` module and its dependencies are designed for Node.js and is available through npm:

`npm install -g apigee-127`

The `-g` option automatically places the `a127` command-line commands in you PATH. If you do not use `-g`, then you need to add the `apigee-127/bin `directory to your PATH manually. 

Typically, the `-g` option places modules in: `/usr/local/lib/node_modules/apigee-127`. 

Here are the dependencies of `a127-magic` and the `a127` CLI:
```json
{
    "dependencies": {
    "apigee-access": "1.0.x",
    "apigeetool": "",
    "async": "0.8.x",
    "commander": "2.3.x",
    "debug": "1.0.x",
    "express": "4.8.x",
    "fs-reverse": "0.0.x",
    "inquirer": "0.5.x",
    "lodash": "2.4.x",
    "ncp": "0.6.x",
    "nodemon": "1.2.x",
    "skeleton": "git://github.com/apigee-127/project-skeleton.git#master",
    "swagger-editor-for-apigee-127": "2.x.x",
        "swagger-tools": "",
    "tail": "0.3.x",
    "underscore": "",
    "usergrid-installer": "0.0.x",
    "volos-swagger": "",
    "volos": "git://github.com/apigee-127/volos.git#master",
    "yamljs": "0.1.x",
    }
}
```