## Quick start

1. Do the `apigee-127` installation described in [Installation](https://github.com/WWitman/docs/wiki/Installation). 
3. Create a root folder for your Apigee 127 projects and cd to that folder.
4. Execute: `a127 project create hello-world`
5. `cd hello-world`
6. `npm install`
7. `a127 project start`
9. In another terminal, run: `curl http://localhost:10010/hello?name=Me`

**Response:**

`Hello, Me`
