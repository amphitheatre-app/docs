+++
title = "Run a PHP application"
description = "Learn how to deploy a PHP application on Amphitheatre"
weight = 4
+++

Getting an application running on Amphitheatre is essentially working out how to
package it as a deployable image. Once packaged it can be deployed to the
Amphitheatre platform.

## The Example Application

You can get the code for the example from [the GitHub
repository](https://github.com/amphitheatre-app/amp-example-php). Just `git clone
https://github.com/amphitheatre-app/amp-example-php` to get a local copy.

The `amp-example-php` application is, as you'd expect for an example, small. It's a PHP
application that print the 'Hello world'. Here's all the code from
`public_html/index.php`:

```php
<?php
$message = "Hello World!";
echo($message);
```

## Running the Application

```sh
cd ~/public_html
php -S localhost:8080
```

Now run the service with curl (in a separate terminal window), by running the
following command (shown with its output):

```
$ curl localhost:8080
Hello, World!
```

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
 Detected PHP app
 Using the following build configuration
         Builder: paketobuildpacks/builder:full
         Buildpacks: paketo-buildpacks/php
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
name = "amp-example-php"
version = "0.0.1"
authors = ["Eguo Wang <wangeguo@gmail.com>"]
edition = "v1"
description = "A simple PHP example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-php"
repository = "https://github.com/amphitheatre-app/amp-example-php"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "php", "getting-started"]
categories = ["example"]

[[build.env]]
name = "BP_PHP_WEB_DIR"
value = "public_html"
```

The amp command will always refer to this file in the current directory if it
exists, specifically for the Character name value at the start. That name will
be used to identify the Character to the Amphitheatre platform. The rest of the
file contains settings to be applied to the Character when it deploys.

See the [Paketo PHP Buildpack
documentation](https://paketo.io/docs/howto/php/)
for more options.

## Deploying to Amphitheatre

To deploy your Character, just run:

```sh
amp run
```

This will lookup our `.amp.toml` file, and get the Character name `amp-example-php`
from there. Then `amp` will start the process of deploying our Character to the
Amphitheatre platform. `amp` will return you to the command line when it's done.

## Arrived at Destination

You have successfully built, deployed your first PHP application on Amphitheatre.
