+++
title = "Aptos"
description = "How to develop smart contract programs on Aptos"
weight = 1
+++

Running Web2 applications on Amphitheatre differs from running Web3 applications, therefore, before deploying Aptos smart contracts, you need to first build the contracts, then deploy them to the DevNet within the cluster to observe the execution of the contracts.

## The Example Application

You can get the code for the example from [the GitHub
repository](https://github.com/amphitheatre-app/amp-example-aptos). Just `git clone
https://github.com/amphitheatre-app/amp-example-aptos` to get a local copy.

The `amp-example-aptos` application is, as you'd expect for an example, small. It's a Aptos
smart contract. 

Here's all the code from `sources/hello_blockchain.move`:

```rust
module hello_blockchain::message {
    use std::error;
    use std::signer;
    use std::string;
    use aptos_framework::event;

    //:!:>resource
    struct MessageHolder has key {
        message: string::String,
    }
    //<:!:resource

    #[event]
    struct MessageChange has drop, store {
        account: address,
        from_message: string::String,
        to_message: string::String,
    }

    /// There is no message present
    const ENO_MESSAGE: u64 = 0;

    #[view]
    public fun get_message(addr: address): string::String acquires MessageHolder {
        assert!(exists<MessageHolder>(addr), error::not_found(ENO_MESSAGE));
        borrow_global<MessageHolder>(addr).message
    }

    public entry fun set_message(account: signer, message: string::String)
    acquires MessageHolder {
        let account_addr = signer::address_of(&account);
        if (!exists<MessageHolder>(account_addr)) {
            move_to(&account, MessageHolder {
                message,
            })
        } else {
            let old_message_holder = borrow_global_mut<MessageHolder>(account_addr);
            let from_message = old_message_holder.message;
            event::emit(MessageChange {
                account: account_addr,
                from_message,
                to_message: copy message,
            });
            old_message_holder.message = message;
        }
    }

    #[test(account = @0x1)]
    public entry fun sender_can_set_message(account: signer) acquires MessageHolder {
        let addr = signer::address_of(&account);
        aptos_framework::account::create_account_for_test(addr);
        set_message(account, string::utf8(b"Hello, Blockchain"));

        assert!(
            get_message(addr) == string::utf8(b"Hello, Blockchain"),
            ENO_MESSAGE
        );
    }
}
```

## Building the Application

As with most Aptos applications, a simple `aptos move compile` will create a binary
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
name = "amp-example-aptos"
version = "0.1.0"
edition = "v1"
description = "A simple Aptos example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-aptos"
repository = "https://github.com/amphitheatre-app/amp-example-aptos"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "aptos", "getting-started"]
categories = ["example"]

[build]
builder = "ghcr.io/amp-buildpacks/move-builder"

[partners]
aptos = { version = "1.9.2", registry = "catalog" }

[build.env]
BP_ENABLE_APTOS_DEPLOY = "false"
# BP_APTOS_DEPLOY_PRIVATE_KEY = "<Your-Deploy-Private-Key>"
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

This will lookup our `.amp.toml` file, and get the Character name `amp-example-aptos`
from there. Then `amp` will start the process of deploying our Character to the
Amphitheatre platform. `amp` will return you to the command line when it's done.

## Arrived at Destination

You have successfully built, deployed your first Aptos application on Amphitheatre.
