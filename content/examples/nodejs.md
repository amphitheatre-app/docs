+++
title = "Run a NodeJS application"
description = "Learn how to deploy a NodeJS application on Amphitheatre"
weight = 3
+++

Getting an application running on Amphitheatre is essentially working out how to
package it as a deployable image. Once packaged it can be deployed to the
Amphitheatre platform.

## The Example Application

Our example will be a basic "hello world" example using Node and Express.

You can get the code for the example from [the GitHub
repository](https://github.com/amphitheatre-app/amp-example-nodejs). Just `git clone
https://github.com/amphitheatre-app/amp-example-nodejs` to get a local copy.
Here's all the code:

```javascript
'use strict';

const express = require('express')
const app = express()

app.use(express.static('public'));
app.get('/hello', (req, res) => res.send('Hello World'))

const port = 3000
app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

We'll call this file `index.js` and run `npm init and npm install express --save`
so we've got the basic node setup.

## Running the Application

Run `node index.js` to start the application

```
$ node index.js
Example app listening on port 3000!
```

And connect to `http://localhost:3000/hello` to confirm that you have a working
Node application. Now to package it up for Amphitheatre.

## Install Amphitheatre

We are ready to start working with Amphitheatre and that means we need `amp`, our CLI
app for managing apps on Amphitheatre. If you've already installed it, carry on. If not,
hop over to [our installation guide](@/installation/_index.md).

## Initialize the Character

To launch an app on Amphitheatre, run `amp init` in the directory with your source
code. This will create and configure a `Character` for you by inspecting your source
code, then prompt you to deploy.

```
$ amp init

Scanning source code
 Detected NodeJS app
 Using the following build configuration
         Builder: heroku/buildpacks:20
Wrote config file .amp.toml
Your Character is ready. run with `amp run`
...
```

First, this command scans your source code to determine how to build a
deployment image as well as identify any other configuration your app needs,
such as secrets and exposed ports.

After your source code is scanned and the results are printed, `amp` creates a
`Character` for you and writes your configuration to a `.amp.toml` file. You'll
then be prompted to build and deploy your character. Once complete, your app
will be running on Amphitheatre.

## Inside .amp.toml

The `.amp.toml` file now contains a default configuration for deploying your
`Character`. If we look at the `.amp.toml` file we can see it in there:

```toml
name = "amp-example-nodejs"
version = "0.0.1"
authors = ["Eguo Wang <wangeguo@gmail.com>"]
edition = "v1"
description = "A simple NodeJS example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-nodejs"
repository = "https://github.com/amphitheatre-app/amp-example-nodejs"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "nodejs", "getting-started"]
categories = ["example"]
```

The amp command will always refer to this file in the current directory if it
exists, specifically for the Character name value at the start. That name will
be used to identify the Character to the Amphitheatre platform. The rest of the
file contains settings to be applied to the Character when it deploys.

## Configuring the Buildpack

The Heroku Nodejs buildpack allows some customization via environment variables.
These may be passed into the build using [Docker build
arguments](https://devcenter.heroku.com/articles/nodejs-support#using-npm-install).

## Deploying to Amphitheatre

To deploy your Character, just run:

```sh
amp run
```

This will lookup our `.amp.toml` file, and get the Character name `amp-example-nodejs`
from there. Then `amp` will start the process of deploying our Character to the
Amphitheatre platform. `amp` will return you to the command line when it's done.

## Arrived at Destination

You have successfully built, deployed your first NodeJS application on Amphitheatre.
