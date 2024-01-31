+++
title = "Sui"
description = "How to develop smart contract programs on Sui"
weight = 1
+++

Running Web2 applications on Amphitheatre differs from running Web3 applications, therefore, before deploying Sui smart contracts, you need to first build the contracts, then deploy them to the DevNet within the cluster to observe the execution of the contracts.

## The Example Application

You can get the code for the example from [the GitHub
repository](https://github.com/amphitheatre-app/amp-example-sui). Just `git clone
https://github.com/amphitheatre-app/amp-example-sui` to get a local copy.

The `amp-example-sui` application is, as you'd expect for an example, small. It's a Sui
smart contract. 

Here's all the code from `sources/example.move`:

```rust
// Copyright (c) Mysten Labs, Inc.
// SPDX-License-Identifier: Apache-2.0

module first_package::example {
    use sui::object::{Self, UID};
    use sui::transfer;
    use sui::tx_context::{Self, TxContext};

    struct Sword has key, store {
        id: UID,
        magic: u64,
        strength: u64,
    }

    struct Forge has key {
        id: UID,
        swords_created: u64,
    }

    /// Module initializer to be executed when this module is published
    fun init(ctx: &mut TxContext) {
        let admin = Forge {
            id: object::new(ctx),
            swords_created: 0,
        };

        // transfer the forge object to the module/package publisher
        transfer::transfer(admin, tx_context::sender(ctx));
    }

    // === Accessors ===

    public fun magic(self: &Sword): u64 {
        self.magic
    }

    public fun strength(self: &Sword): u64 {
        self.strength
    }

    public fun swords_created(self: &Forge): u64 {
        self.swords_created
    }

    /// Constructor for creating swords
    public fun new_sword(
        forge: &mut Forge,
        magic: u64,
        strength: u64,
        ctx: &mut TxContext,
    ): Sword {
        forge.swords_created = forge.swords_created + 1;
        Sword {
            id: object::new(ctx),
            magic: magic,
            strength: strength,
        }
    }

    // === Tests ===
    #[test_only] use sui::test_scenario as ts;

    #[test_only] const ADMIN: address = @0xAD;
    #[test_only] const ALICE: address = @0xA;
    #[test_only] const BOB: address = @0xB;

    #[test]
    public fun test_module_init() {
        let ts = ts::begin(@0x0);

        // first transaction to emulate module initialization.
        {
            ts::next_tx(&mut ts, ADMIN);
            init(ts::ctx(&mut ts));
        };

        // second transaction to check if the forge has been created
        // and has initial value of zero swords created
        {
            ts::next_tx(&mut ts, ADMIN);

            // extract the Forge object
            let forge: Forge = ts::take_from_sender(&ts);

            // verify number of created swords
            assert!(swords_created(&forge) == 0, 1);

            // return the Forge object to the object pool
            ts::return_to_sender(&ts, forge);
        };

        ts::end(ts);
    }

    #[test]
    fun test_sword_transactions() {
        let ts = ts::begin(@0x0);

        // first transaction to emulate module initialization
        {
            ts::next_tx(&mut ts, ADMIN);
            init(ts::ctx(&mut ts));
        };

        // second transaction executed by admin to create the sword
        {
            ts::next_tx(&mut ts, ADMIN);
            let forge: Forge = ts::take_from_sender(&ts);
            // create the sword and transfer it to the initial owner
            let sword = new_sword(&mut forge, 42, 7, ts::ctx(&mut ts));
            transfer::public_transfer(sword, ALICE);
            ts::return_to_sender(&ts, forge);
        };

        // third transaction executed by the initial sword owner
        {
            ts::next_tx(&mut ts, ALICE);
            // extract the sword owned by the initial owner
            let sword: Sword = ts::take_from_sender(&ts);
            // transfer the sword to the final owner
            transfer::public_transfer(sword, BOB);
        };

        // fourth transaction executed by the final sword owner
        {
            ts::next_tx(&mut ts, BOB);
            // extract the sword owned by the final owner
            let sword: Sword = ts::take_from_sender(&ts);
            // verify that the sword has expected properties
            assert!(magic(&sword) == 42 && strength(&sword) == 7, 1);
            // return the sword to the object pool (it cannot be dropped)
            ts::return_to_sender(&ts, sword)
        };

        ts::end(ts);
    }
}
```

## Building the Application

As with most Sui applications, a simple `sui move build` will create a binary
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
name = "amp-example-sui"
version = "0.1.0"
edition = "v1"
description = "A simple Sui example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-sui"
repository = "https://github.com/amphitheatre-app/amp-example-sui"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "sui", "getting-started"]
categories = ["example"]

[build]
builder = "ghcr.io/amp-buildpacks/move-builder"

[partners]
sui = { version = "stable", registry = "catalog" }

[build.env]
BP_ENABLE_SUI_DEPLOY = "false"
# BP_SUI_DEPLOY_PRIVATE_KEY = "<Your-Deploy-Private-Key>"
```

The amp command will always refer to this file in the current directory if it
exists, specifically for the Character name value at the start. That name will
be used to identify the Character to the Amphitheatre platform. The rest of the
file contains settings to be applied to the Character when it deploys.

See the [move-builder
documentation](https://github.com/amp-buildpacks/move-builder)
for more options.

## Deploying to Amphitheatre

To deploy your Character, just run:

```sh
amp run
```

This will lookup our `.amp.toml` file, and get the Character name `amp-example-sui`
from there. Then `amp` will start the process of deploying our Character to the
Amphitheatre platform. `amp` will return you to the command line when it's done.

## Arrived at Destination

You have successfully built, deployed your first Sui application on Amphitheatre.
