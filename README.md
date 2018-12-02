![](http://res.cloudinary.com/hashnode/image/upload/w_200/v1466495663/static_imgs/mern/v2/mernio-logo.png)

# Proto_app 
![title](https://travis-ci.org/Hashnode/mern-starter.svg?branch=v2.0.0)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

we use MERN as our seed.

MERN is a scaffolding tool which makes it easy to build isomorphic apps using Mongo, Express, React and NodeJS. It minimises the setup time and gets you up to speed using proven technologies.

- [Website](http://mern.io)
- [Documentation](http://mern.io/documentation.html)
- [Discussions](https://hashnode.com/n/mern)

**Note : Please make sure your MongoDB is running.** For MongoDB installation guide see [this](https://docs.mongodb.org/v3.0/installation/). Also `npm6` is required to install dependencies properly.

## Available Commands

1. `npm run start` - starts the development server with hot reloading enabled

2. `npm run bs` - bundles the code and starts the production server

3. `npm run test` - start the test runner

4. `npm run watch:test` - start the test runner with watch mode

5. `npm run cover` - generates test coverage report

6. `npm run lint` - runs linter to check for lint errors



## Misc

### Importing Assets
Assets can be kept where you want and can be imported into your js files or css files. Those fill be served by webpack in development mode and copied to the dist folder during production.

### ES6 support
We use babel to transpile code in both server and client with `stage-0` plugin. So, you can use both ES6 and experimental ES7 features.

### Docker
There are docker configurations for both development and production.

To run docker for development:
```sh
docker-compose build # re-run after changing dependencies
docker-compose up
```
or, if you want to override the web port:
```sh
WEB_PORT=<your_custom_port> docker-compose up
```

To run docker for production:
```sh
docker-compose -f docker-compose-production.yml up --build
```

To reset the database:
```sh
docker-compose down --volumes
```

### Modifying Generators

#### mern.json
It contains a blueprints array. Each object in it is the config for a generator. A blueprint config contains the name, description, usage, and files array. An example blueprint config
```json
{
  "name": "dumb-s",
  "description": "Generates a dumb react component in shared components",
  "usage": "dumb-s [component-name]",
  "files": [
    {
      "blueprint-path": "config/blueprints/dumb-component.ejs",
      "target-path": "client/components/<%= helpers.capitalize(name) %>.js"
    }
  ]
}
```

A file object contains

1. `blueprint-path` - location of the blueprint file

2. `target-path` - location where the file should be generated

3. `parent-path` - optional parameter, used if you want to generate the file inside an already existing folder in your project.

Also, `target-path` supports [ejs](https://github.com/mde/ejs) and the following variables will be passed while rendering,

1. `name` - `<component-name>` input from user

2. `parent` - in particular special cases where you need to generate files inside an already existing folder, you can obtain this parent variable from the user. A config using that will look like,
    ```json
    {
      "name": "dumb-m",
      "description": "Generates a dumb react component in a module directory",
      "usage": "dumb-m <module-name>/<component-name>",
      "files": [
        {
          "blueprint-path": "config/blueprints/dumb-component.ejs",
          "parent-path": "client/modules/<%= helpers.capitalize(parent) %>",
          "target-path": "components/<%= helpers.capitalize(name) %>/<%= helpers.capitalize(name) %>.js"
        }
      ]
    }
    ```
    Here, notice the usage. In `<module-name>/<component-name>`, `<module-name>` will be passed as `parent` and `<component-name>` will be passed as `<name>`.

3. `helpers` - an helper object is passed which include common utility functions. For now, it contains `capitalize`. If you want to add more, send a PR to [mern-cli](https://github.com/Hashnode/mern-cli).

#### Blueprint files
Blueprints are basically [ejs](https://github.com/mde/ejs) templates which are rendered with the same three variables (`name`, optional `parent` and `helpers` object) as above.

### Caveats

#### FOUC (Flash of Unstyled Content)
To make the hot reloading of CSS work, we are not extracting CSS in development. Ideally, during server rendering, we will be extracting CSS, and we will get a .css file, and we can use it in the html template. That's what we are doing in production.

In development, after all scripts get loaded, react loads the CSS as BLOBs. That's why there is a second of FOUC in development.

#### Client and Server Markup Mismatch
This warning is visible only on development and totally harmless. This occurs to hash difference in `react-router`. To solve it, react router docs asks you to use `match` function. If we use `match`, `react-hot-reloader` stops working.
