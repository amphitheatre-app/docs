+++
title = "Sui"
description = "如何在 Sui 上开发智能合约程序"
weight = 1
+++

在 Amphitheatre 上运行 Web2 应用程序与 Web3 应用程序有所差异，因此，部署 Sui 智能合约之前，您需要先构建合约，然后，将合约部署到集群内的 DevNet 上，以此查看合约的运行情况。

## 示例应用程序

您可以从 [GitHub 存储库](https://github.com/amphitheatre-app/amp-example-sui) 获
取示例的代码。只需运行以下命令以获取本地副本：`git clone
https://github.com/amphitheatre-app/amp-example-sui`。

`amp-example-sui` 应用程序是一个小型示例，正如您所期望的那样。它是一个 Sui 智能合约。

`sources/example.move` 代码如下所示：

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

## 构建应用程序

与大多数 Sui 应用程序一样，简单的 `sui move build` 将创建一个可运行的二进制文件。因此，
原始应用程序可以正常运行。现在，将其打包以供 Amphitheatre 使用。

## 安装 Amphitheatre

我们已准备好开始使用 Amphitheatre，并且这意味着我们需要 `amp`，这是用于管理
Amphitheatre 上应用程序的 CLI 应用程序。如果您已经安装了它，请继续。如果没有，请
跳转到 [我们的安装指南](@/installation/_index.zh.md)。

## 在 .amp.toml 中

`.amp.toml` 文件现在包含用于部署您的 `Character` 的默认配置。如果我们查看
`.amp.toml` 文件，可以在其中看到：

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

如果该文件存在，amp 命令将始终引用当前目录中的此文件，特别是开始时的 Character
名称值。该名称将用于标识 Character 到 Amphitheatre 平台。文件的其余部分包含部署
Character 时要应用的设置。

有关更多选项，请参阅 [move-builder 文档](https://github.com/amp-buildpacks/move-builder)。

## 部署到 Amphitheatre

要部署您的 Character，请运行：

```sh
amp run
```

这将查找我们的 `.amp.toml` 文件，并从中获取 Character 名称 `amp-example-sui`。然
后，`amp` 将开始部署 Character 到 Amphitheatre 平台。`amp` 将在完成时返回到命令
