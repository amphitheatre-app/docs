+++
title = "Starknet"
description = "如何在 Starknet 上开发智能合约程序"
weight = 1
+++

在 Amphitheatre 上运行 Web2 应用程序与 Web3 应用程序有所差异，因此，部署 Starknet 智能合约之前，您需要先构建合约，然后，将合约部署到集群内的 DevNet 上，以此查看合约的运行情况。

## 示例应用程序

您可以从 [GitHub 存储库](https://github.com/amphitheatre-app/amp-example-starknet) 获
取示例的代码。只需运行以下命令以获取本地副本：`git clone
https://github.com/amphitheatre-app/amp-example-starknet`。

`amp-example-starknet` 应用程序是一个小型示例，正如您所期望的那样。它是一个 Starknet 智能合约。

`src/lib.cairo` 代码如下所示：

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

## 构建应用程序

与大多数 Starknet 应用程序一样，简单的 `scarb build` 将创建一个可运行的二进制文件。因此，
原始应用程序可以正常运行。现在，将其打包以供 Amphitheatre 使用。

## 安装 Amphitheatre

我们已准备好开始使用 Amphitheatre，并且这意味着我们需要 `amp`，这是用于管理
Amphitheatre 上应用程序的 CLI 应用程序。如果您已经安装了它，请继续。如果没有，请
跳转到 [我们的安装指南](@/installation/_index.zh.md)。

## 在 .amp.toml 中

`.amp.toml` 文件现在包含用于部署您的 `Character` 的默认配置。如果我们查看
`.amp.toml` 文件，可以在其中看到：

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

如果该文件存在，amp 命令将始终引用当前目录中的此文件，特别是开始时的 Character
名称值。该名称将用于标识 Character 到 Amphitheatre 平台。文件的其余部分包含部署
Character 时要应用的设置。

有关更多选项，请参阅 [cairo-builder 文档](https://github.com/amp-buildpacks/cairo-builder)。

## 部署到 Amphitheatre

要部署您的 Character，请运行：

```sh
amp run
```

这将查找我们的 `.amp.toml` 文件，并从中获取 Character 名称 `amp-example-starknet`。然
后，`amp` 将开始部署 Character 到 Amphitheatre 平台。`amp` 将在完成时返回到命令
