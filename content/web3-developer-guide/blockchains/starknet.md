+++
title = "Starknet"
description = "How to develop smart contract programs on Starknet"
weight = 1
+++

Running Web2 applications on Amphitheatre differs from running Web3 applications, therefore, before deploying Starknet smart contracts, you need to first build the contracts, then deploy them to the DevNet within the cluster to observe the execution of the contracts.

## The Example Application

You can get the code for the example from [the GitHub
repository](https://github.com/amphitheatre-app/amp-example-starknet). Just `git clone
https://github.com/amphitheatre-app/amp-example-starknet` to get a local copy.

The `amp-example-starknet` application is, as you'd expect for an example, small. It's a Starknet
smart contract. 

Here's all the code from `src/lib.cairo`:

```rust
use core::traits::TryInto;
use starknet::ContractAddress;

#[starknet::interface]
trait IData<T> {
    fn get_data(self: @T) -> felt252;
    fn set_data(ref self: T, new_value: felt252);
    fn other_func(self: @T, other_contract: ContractAddress) -> felt252;
}

#[starknet::interface]
trait OwnableTrait<T> {
    fn transfer_ownership(ref self: T, new_owner: ContractAddress);
    fn owner(self: @T) -> ContractAddress;
}

#[starknet::contract]
mod ownable {
    use starknet::get_caller_address;
    use super::{ContractAddress, IData, IDataDispatcherTrait, IDataDispatcher, OwnableTrait};

    #[storage]
    struct Storage {
        owner: ContractAddress,
        data: felt252,
    }

    #[event]
    #[derive(Drop, starknet::Event)]
    enum Event {
        OwnershipTransferred: OwnershipTransferred,
    }

    #[derive(Drop, starknet::Event)]
    struct OwnershipTransferred {
        #[key]
        prev_owner: ContractAddress,
        #[key]
        new_owner: ContractAddress,
    }

    #[constructor]
    fn constructor(ref self: ContractState, initial_owner: ContractAddress) {
        self.owner.write(initial_owner);
        self.data.write(1);
    // Any variable of the storage that is not initialized
    // will have default value -> data = 0.
    }

    #[abi(embed_v0)]
    impl OwnableDataImpl of IData<ContractState> {
        fn other_func(self: @ContractState, other_contract: ContractAddress) -> felt252 {
            IDataDispatcher { contract_address: other_contract }.get_data()
        }

        fn get_data(self: @ContractState) -> felt252 {
            self.data.read()
        }

        fn set_data(ref self: ContractState, new_value: felt252) {
            self.only_owner();
            self.data.write(new_value);
        }
    }

    #[abi(embed_v0)]
    impl OwnableTraitImpl of OwnableTrait<ContractState> {
        fn transfer_ownership(ref self: ContractState, new_owner: ContractAddress) {
            self.only_owner();
            let prev_owner = self.owner.read();
            self.owner.write(new_owner);

            self.emit(OwnershipTransferred { prev_owner, new_owner });
        }

        fn owner(self: @ContractState) -> ContractAddress {
            self.owner.read()
        }
    }

    #[generate_trait]
    impl PrivateMethods of PrivateMethodsTrait {
        fn only_owner(self: @ContractState) {
            let caller = get_caller_address();
            assert(caller == self.owner.read(), 'Caller is not the owner');
        }
    }
}

#[cfg(test)]
mod tests {
    use ownable_starknet::ownable;
    use ownable_starknet::{OwnableTraitDispatcher, OwnableTraitDispatcherTrait};
    use starknet::{ContractAddress, Into, TryInto, OptionTrait};
    use starknet::syscalls::deploy_syscall;
    use result::ResultTrait;
    use array::{ArrayTrait, SpanTrait};

    #[test]
    #[available_gas(10_000_000)]
    fn unit_test() {
        let admin_address: ContractAddress = 'admin'.try_into().unwrap();
        let mut calldata = array![admin_address.into()];
        let (address0, _) = deploy_syscall(
            ownable::TEST_CLASS_HASH.try_into().unwrap(), 0, calldata.span(), false
        )
            .unwrap();
        let mut contract0 = OwnableTraitDispatcher { contract_address: address0 };

        assert(contract0.owner() == admin_address, 'Wrong owner');
    }
}
```

## Building the Application

As with most Starknet applications, a simple `scarb build` will create a binary
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
name = "amp-example-starknet"
version = "0.1.0"
edition = "v1"
description = "A simple Starknet example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-starknet"
repository = "https://github.com/amphitheatre-app/amp-example-starknet"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "starknet", "getting-started"]
categories = ["example"]

[build]
builder = "ghcr.io/amp-buildpacks/cairo-builder"

[partners]
starknet = { version = "stable", registry = "catalog" }

[build.env]
BP_ENABLE_STARKNET_DEPLOY = "false"
# BP_STARKNET_DEPLOY_PRIVATE_KEY = "<Your-Deploy-Private-Key>"
# BP_STARKNET_DEPLOY_WALLET_ADDRESS = "<Your-Deploy-Wallet-Address>"
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

This will lookup our `.amp.toml` file, and get the Character name `amp-example-starknet`
from there. Then `amp` will start the process of deploying our Character to the
Amphitheatre platform. `amp` will return you to the command line when it's done.

## Arrived at Destination

You have successfully built, deployed your first Starknet application on Amphitheatre.
