+++
title = "Run a Static Website"
description = "Learn how to deploy a static site on Amphitheatre"
weight = 8
+++

Getting an application running on Amphitheatre is essentially working out how to
package it as a deployable image. Once packaged it can be deployed to the
Amphitheatre platform.

To be fair, a static site isn’t an app. So we’re really talking about deploying
an app to serve some static content.

In this demonstration, we’ll use
[goStatic](https://github.com/PierreZ/goStatic), a tiny web server written in Go
that lets us serve static files with very little configuration. We’ll provide a
Dockerfile and our content for Amphitheatre to transmogrify into a web server running in
a VM.

## The Example Application

You can get the code for the example from [the GitHub
repository](https://github.com/amphitheatre-app/amp-example-static). Just `git clone
https://github.com/amphitheatre-app/amp-example-static` to get a local copy.

Alternatively, you can create all the files manually as you work through this
guide.

## Putting the app together

At this point, if you have a local clone of the `amp-example-static` repository,
you could go ahead and run `amp run` from its root directory and get the static
site deployed without further ado. But that wouldn’t be very illuminating. Let’s
go through what’s included in the example repository and why.

If you cloned the repository, your new app already has its own directory.
Otherwise, create one. This isn’t just for tidiness, or for letting `amp` detect
your app by the `.amp.toml` in the working directory (although these are good
reasons). It also ensures that no extra files get included in the [build
context](https://docs.docker.com/engine/reference/commandline/build/) when the
Docker image gets built.

We’ll do everything from within this directory:

```sh
cd amp-example-static
```

### The Site

Our example will be a simple static site. That can be as trivial as a single
`index.html` file. Let’s make it only slightly more complicated by writing two
html files and having them link to each other.

Put these html files into a subdirectory of their own, called `public`. Files in
this directory are the ones our `goStatic` server will serve. Create the
`amp-example-static/public` directory if needed.

Here’s `index.html`, which is the landing page:

```html
<html>
  <head>
    <title>
      Hello from Amphitheatre
    </title>
  </head>
  <body>
    <h1>Hello from Amphitheatre with a static web site</h1>
      <p>Or <a href="goodbye.html">goodbye.</a></p>
  </body>
</html>
```

Here’s `goodbye.html`.

```html
<html>
  <head>
    <title>
      Still Hello from Amphitheatre
    </title>
  </head>
  <body>
    <h1>You say goodbye</h1>
      <p>But I say <a href="index.html">hello.</a></p>
  </body>
</html>
```

### The Dockerfile

`goStatic` is designed to run in a container, and the
[image](https://hub.docker.com/r/pierrezemb/gostatic) is available at Docker
Hub. This is super convenient for us, because Amphitheatre apps need container
images too!

We can use the goStatic image as a base image. We just have to copy our site’s
files to /srv/http/ in the image.

Here’s our Dockerfile to do that:

```Dockerfile
FROM pierrezemb/gostatic
COPY ./public/ /srv/http/
```

The Dockerfile should be placed in the working directory (here, `amp-example-static`).

## Install Amphitheatre

We are ready to start working with Amphitheatre and that means we need `amp`, our CLI
app for managing apps on Amphitheatre. If you've already installed it, carry on. If not,
hop over to [our installation guide](@/installation/_index.md).

## Initialize the Character

To launch an app on Amphitheatre, run `amp init` in the directory with your source
code. This will create and configure a `Character` for you by inspecting your source
code, then prompt you to deploy.

```sh
$ amp init

Scanning source code
 Detected a Dockerfile app
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
name = "amp-example-static"
version = "0.0.1"
authors = ["Eguo Wang <wangeguo@gmail.com>"]
edition = "v1"
description = "A static website"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-static"
repository = "https://github.com/amphitheatre-app/amp-example-static"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "static", "getting-started"]
categories = ["example"]

[build]
dockerfile = "Dockerfile"
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

This will lookup our `.amp.toml` file, and get the Character name `amp-example-static`
from there. Then `amp` will start the process of deploying our Character to the
Amphitheatre platform. `amp` will return you to the command line when it's done.

## Arrived at Destination

You have successfully built, deployed your static site on Amphitheatre.
