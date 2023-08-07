+++
title = "Run a Golang application"
description = "Learn how to deploy a Golang application on Amphitheatre"
weight = 1
+++

Getting an application running on Amphitheatre is essentially working out how to
package it as a deployable image. Once packaged it can be deployed to the
Amphitheatre platform.

## The Example Application

You can get the code for the example from [the GitHub
repository](https://github.com/amphitheatre-app/amp-example-go). Just `git clone
https://github.com/amphitheatre-app/amp-example-go` to get a local copy.

The `amp-example-go` application is, as you'd expect for an example, small. It's a Go
application that loop print the 'Hello world'. Here's all the code from `main.go`:

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	for {
		fmt.Println("Hello world!")
		time.Sleep(time.Second * 1)
	}
}
```

## Building the Application

As with most Go applications, a simple `go build` will create a binary
which we can run. So, the raw application works. Now to package
it up for Amphitheatre.

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
 Detected Go app
 Using the following build configuration
         Builder: paketobuildpacks/builder:base
         Buildpacks: gcr.io/paketo-buildpacks/go
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
name = "amp-example-go"
version = "0.0.2"
authors = ["Eguo Wang <wangeguo@gmail.com>"]
edition = "v1"
description = "A simple Golang example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-go"
repository = "https://github.com/amphitheatre-app/amp-example-go"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "golang", "getting-started"]
categories = ["example"]
```

The amp command will always refer to this file in the current directory if it
exists, specifically for the Character name value at the start. That name will
be used to identify the Character to the Amphitheatre platform. The rest of the
file contains settings to be applied to the Character when it deploys.

See the [Paketo Go Buildpack
documentation](https://paketo.io/docs/howto/go/)
for more options.

## Deploying to Amphitheatre

To deploy your Character, just run:

```sh
amp run
```

This will lookup our `.amp.toml` file, and get the Character name `amp-example-go`
from there. Then `amp` will start the process of deploying our Character to the
Amphitheatre platform. `amp` will return you to the command line when it's done.

## Arrived at Destination

You have successfully built, deployed your first Go application on Amphitheatre.
