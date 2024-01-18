+++
title = "Run a Aleo application"
description = "Learn how to deploy a Aleo application on Amphitheatre"
weight = 1
+++

Getting an application running on Amphitheatre is essentially working out how to
package it as a deployable image. Once packaged it can be deployed to the
Amphitheatre platform.

## The Example Application

You can get the code for the example from [the GitHub
repository](https://github.com/amphitheatre-app/amp-example-aleo). Just `git clone
https://github.com/amphitheatre-app/amp-example-aleo` to get a local copy.

The `amp-example-aleo` application is, as you'd expect for an example, small. It's a Aleo
application that loop print the '3u32'. 

Here's all the code from `src/main.leo`:

```rust
// The 'snarkos' program.
program snarkos.aleo {
    transition main(public a: u32, b: u32) -> u32 {
        let c: u32 = a + b;
        return c;
    }
}
```

Here's all the code from `inputs/snarkos.in`:

```toml
// The program input for snarkos/src/main.leo
[main]
public a: u32 = 1u32;
b: u32 = 2u32;
```

## Building the Application

As with most Aleo applications, a simple `leo build` will create a binary
which we can run. So, the raw application works. Now to package
it up for Amphitheatre.

## Install Amphitheatre

We are ready to start working with Amphitheatre and that means we need `amp`, our CLI
app for managing apps on Amphitheatre. If you've already installed it, carry on. If not,
hop over to [our installation guide](@/installation/_index.md).

## Inside .amp.toml

The `.amp.toml` file now contains a default configuration for deploying your
`Character`. If we look at the `.amp.toml` file we can see it in there:

```toml
name = "amp-example-aleo"
version = "0.1.0"
edition = "v1"
description = "A simple Aleo example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-aleo"
repository = "https://github.com/amphitheatre-app/amp-example-aleo"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "aleo", "getting-started"]
categories = ["example"]

[build]
builder = "ghcr.io/amp-buildpacks/leo-builder:0.1.0"

[partners]
snarkos = { version = "latest", registry = "catalog" }
```

The amp command will always refer to this file in the current directory if it
exists, specifically for the Character name value at the start. That name will
be used to identify the Character to the Amphitheatre platform. The rest of the
file contains settings to be applied to the Character when it deploys.

See the [leo-builder
documentation](https://github.com/amp-buildpacks/leo-builder)
for more options.

## Deploying to Amphitheatre

To deploy your Character, just run:

```sh
amp run
```

This will lookup our `.amp.toml` file, and get the Character name `amp-example-aleo`
from there. Then `amp` will start the process of deploying our Character to the
Amphitheatre platform. `amp` will return you to the command line when it's done.

## Arrived at Destination

You have successfully built, deployed your first Aleo application on Amphitheatre.
