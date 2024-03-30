+++
title = "Aptos"
description = "如何在 Aptos 上开发智能合约程序"
weight = 1
+++

在 Amphitheatre 上运行 Web2 应用程序与 Web3 应用程序有所差异，因此，部署 Aptos 智能合约之前，您需要先构建合约，然后，将合约部署到集群内的 DevNet 上，以此查看合约的运行情况。

## 示例应用程序

您可以从 [GitHub 存储库](https://github.com/amphitheatre-app/amp-example-aptos) 获
取示例的代码。只需运行以下命令以获取本地副本：`git clone
https://github.com/amphitheatre-app/amp-example-aptos`。

`amp-example-aptos` 应用程序是一个小型示例，正如您所期望的那样。它是一个 Aptos 智能合约。

`sources/hello_blockchain.move` 代码如下所示：

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

## 构建应用程序

与大多数 Aptos 应用程序一样，简单的 `aptos move compile` 将创建一个可运行的二进制文件。因此，
原始应用程序可以正常运行。现在，将其打包以供 Amphitheatre 使用。

## 安装 Amphitheatre

我们已准备好开始使用 Amphitheatre，并且这意味着我们需要 `amp`，这是用于管理
Amphitheatre 上应用程序的 CLI 应用程序。如果您已经安装了它，请继续。如果没有，请
跳转到 [我们的安装指南](@/installation/_index.zh.md)。

## 在 .amp.toml 中

`.amp.toml` 文件现在包含用于部署您的 `Character` 的默认配置。如果我们查看
`.amp.toml` 文件，可以在其中看到：

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

如果该文件存在，amp 命令将始终引用当前目录中的此文件，特别是开始时的 Character
名称值。该名称将用于标识 Character 到 Amphitheatre 平台。文件的其余部分包含部署
Character 时要应用的设置。

有关更多选项，请参阅 [move-builder 文档](https://github.com/amp-buildpacks/move-builder)。

## 部署到 Amphitheatre

要部署您的 Character，请运行：

```sh
amp run
```

这将查找我们的 `.amp.toml` 文件，并从中获取 Character 名称 `amp-example-aptos`。然
后，`amp` 将开始部署 Character 到 Amphitheatre 平台。`amp` 将在完成时返回到命令
