+++
title = "Solana"
description = "How to develop smart contract programs on Solana"
weight = 1
+++

Running Web2 applications on Amphitheatre differs from running Web3 applications, therefore, before deploying Solana smart contracts, you need to first build the contracts, then deploy them to the DevNet within the cluster to observe the execution of the contracts.

## The Example Application

You can get the code for the example from [the GitHub
repository](https://github.com/amphitheatre-app/amp-example-solana). Just `git clone
https://github.com/amphitheatre-app/amp-example-solana` to get a local copy.

The `amp-example-solana` application is, as you'd expect for an example, small. It's a Solana
smart contract. 

Here's all the code from `src/lib.rs`:

```rust
use solana_program::{
    account_info::AccountInfo, 
    entrypoint, 
    entrypoint::ProgramResult, 
    msg, 
    pubkey::Pubkey,
};

// declare and export the program's entrypoint
entrypoint!(process_instruction);
 
// program entrypoint's implementation
pub fn process_instruction(
    _program_id: &Pubkey,
    _accounts: &[AccountInfo],
    _instruction_data: &[u8]
) -> ProgramResult {
    // log a message to the blockchain
    msg!("Hello, world!");
 
    // gracefully exit the program
    Ok(())
}
```

## Building the Application

As with most Solana applications, a simple `cargo build-sbf` will create a binary
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
name = "amp-example-solana"
version = "0.1.0"
edition = "v1"
description = "A simple Solana example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-solana"
repository = "https://github.com/amphitheatre-app/amp-example-solana"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "solana", "getting-started"]
categories = ["example"]

[build]
buildpacks = ["ghcr.io/amp-buildpacks/solana"]

[build.env]
BP_ENABLE_DEPLOY_SOLANA_CONTRACT = "true"
# BP_SOLANA_DEPLOY_NETWORK = "devnet"
# BP_WALLET_KEYPAIR = ""
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

This will lookup our `.amp.toml` file, and get the Character name `amp-example-solana`
from there. Then `amp` will start the process of deploying our Character to the
Amphitheatre platform. `amp` will return you to the command line when it's done.

## Arrived at Destination

You have successfully built, deployed your first Solana application on Amphitheatre.