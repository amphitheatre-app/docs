+++
title = "Fuel"
description = "How to develop smart contract programs on Fuel"
weight = 1
+++

Running Web2 applications on Amphitheatre differs from running Web3 applications, therefore, before deploying Fuel smart contracts, you need to first build the contracts, then deploy them to the DevNet within the cluster to observe the execution of the contracts.

## The Example Application

You can get the code for the example from [the GitHub
repository](https://github.com/amphitheatre-app/amp-example-sway). Just `git clone
https://github.com/amphitheatre-app/amp-example-sway` to get a local copy.

The `amp-example-sway` application is, as you'd expect for an example, small. It's a Fuel
smart contract. 

Here's all the code from `src/main.sw`:

```rust
contract;
 
storage {
    counter: u64 = 0,
}
 
abi Counter {
    #[storage(read, write)]
    fn increment();
 
    #[storage(read)]
    fn count() -> u64;
}
 
impl Counter for Contract {
    #[storage(read)]
    fn count() -> u64 {
        storage.counter.read()
    }
 
    #[storage(read, write)]
    fn increment() {
        let incremented = storage.counter.read() + 1;
        storage.counter.write(incremented);
    }
}
```

## Building the Application

As with most Fuel applications, a simple `forc build` will create a binary
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
name = "amp-example-sway"
version = "0.1.0"
edition = "v1"
description = "A simple Fuel example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-sway"
repository = "https://github.com/amphitheatre-app/amp-example-sway"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "fuel", "sway", "getting-started"]
categories = ["example"]

[build]
builder = "ghcr.io/amp-buildpacks/sway-builder"

[partners]
fuel = { version = "stable", registry = "catalog" }

[build.env]
BP_ENABLE_FORC_DEPLOY = "true"
BP_HOME = "/layers/amp-buildpacks_sway/forc-amd64/fuel"
```

The amp command will always refer to this file in the current directory if it
exists, specifically for the Character name value at the start. That name will
be used to identify the Character to the Amphitheatre platform. The rest of the
file contains settings to be applied to the Character when it deploys.

See the [sway-builder
documentation](https://github.com/amp-buildpacks/sway-builder)
for more options.

## Deploying to Amphitheatre

To deploy your Character, just run:

```sh
amp run
```

This will lookup our `.amp.toml` file, and get the Character name `amp-example-sway`
from there. Then `amp` will start the process of deploying our Character to the
Amphitheatre platform. `amp` will return you to the command line when it's done.

## Arrived at Destination

You have successfully built, deployed your first Fuel application on Amphitheatre.
