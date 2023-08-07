+++
title = "Run a Python application"
description = "Learn how to deploy a Python application on Amphitheatre"
weight = 5
+++

Getting an application running on Amphitheatre is essentially working out how to
package it as a deployable image. Once packaged it can be deployed to the
Amphitheatre platform.

## The Example Application

You can get the code for the example from [the GitHub
repository](https://github.com/amphitheatre-app/amp-example-go). Just `git clone
https://github.com/amphitheatre-app/amp-example-go` to get a local copy.

The `amp-example-python` application is, as you'd expect for an example, small.
It's a Python application that uses the Flask web framework. Here's all the code
form `web.py`:


```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def index():
  return 'hello, world'
```

You will need to install Flask itself, or at least set up virtual environments
as recommended in the [Flask Install
guide](https://flask.palletsprojects.com/en/1.1.x/installation/#virtual-environments).

Once you have activated the virtual environment, run:

```sh
python -m pip install -r requirements.txt
```

This will load Flask and other required packages. One of those packages will be
gunicorn which isn't a Flask dependency, but will be used when we deploy the app
to Amphitheatre.


## Testing the Application

Flask apps are run with the `flask run` command, but before you do that, you need
to set an environment variable `FLASK_APP` to say which app you want to run.

```
$ FLASK_APP=hellofly flask run

* Serving Flask app "web"
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

This will run our web app and you should be able to connect to it locally
on `localhost:5000`.

Now, let's move on to deploying this app to Amphitheatre.

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
 Detected Python app
 Using the following build configuration
         Builder: paketobuildpacks/builder:base
         Buildpacks: gcr.io/paketo-buildpacks/python
Wrote config file .amp.toml
Your Character is ready. run with `amp run`
...
```

First, this command scans your source code to determine how to build a
deployment image as well as identify any other configuration your app needs,
such as secrets and exposed ports.

After your source code is scanned and the results are printed, `amp` creates a
`Character` for you and writes your configuration to a `.amp.toml` file. You'll
then be prompted to build and deploy your character.

One thing to know about the builtin Python builder is that it will automatically
copy over the contents of the directory to the deployable image. This is how you
can move static assets such as templates and other files to your application.
The other thing to know is that it uses a Procfile to run the application;
Procfiles are used on other platforms to deploy Python applications so we keep
it simple. The Procfile contains instructions for starting the application.
Here's the contents of ours:

```
web: gunicorn web:app
```

This says the web component of the application is served by gunicorn (which we
mentioned earlier when talking about dependencies) and that should run the
web Flask app as we set up for Flask.

Once complete, your app will be running on Amphitheatre.

## Inside .amp.toml

The `.amp.toml` file now contains a default configuration for deploying your
`Character`. If we look at the `.amp.toml` file we can see it in there:

```toml
name = "amp-example-python"
version = "0.0.1"
authors = ["Eguo Wang <wangeguo@gmail.com>"]
edition = "v1"
description = "A simple Python example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-python"
repository = "https://github.com/amphitheatre-app/amp-example-python"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "python", "getting-started"]
categories = ["example"]
```

The amp command will always refer to this file in the current directory if it
exists, specifically for the Character name value at the start. That name will
be used to identify the Character to the Amphitheatre platform. The rest of the
file contains settings to be applied to the Character when it deploys.

## Deploying to Amphitheatre

To deploy your Character, just run:

```sh
amp run
```

This will lookup our `.amp.toml` file, and get the Character name `amp-example-python`
from there. Then `amp` will start the process of deploying our Character to the
Amphitheatre platform. `amp` will return you to the command line when it's done.

## Arrived at Destination

You have successfully built, deployed your first Python application on Amphitheatre.
