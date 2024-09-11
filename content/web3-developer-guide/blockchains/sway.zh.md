+++
title = "Sway"
description = "如何在 Sway 上开发智能合约程序"
weight = 1
+++

在 Amphitheatre 上运行 Web2 应用程序与 Web3 应用程序有所差异，因此，部署 Sway 智能合约之前，您需要先构建合约，然后，将合约部署到集群内的 DevNet 上，以此查看合约的运行情况。

## 示例应用程序

您可以从 [GitHub 存储库](https://github.com/amphitheatre-app/amp-example-sway) 获
取示例的代码。只需运行以下命令以获取本地副本：`git clone
https://github.com/amphitheatre-app/amp-example-sway`。

`amp-example-sway` 应用程序是一个小型示例，正如您所期望的那样。它是一个 Sway 智能合约。

`src/main.sw` 代码如下所示：

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

## 构建应用程序

与大多数 Sway 应用程序一样，简单的 `forc build` 将创建一个可运行的二进制文件。因此，
原始应用程序可以正常运行。现在，将其打包以供 Amphitheatre 使用。

## 安装 Amphitheatre

我们已准备好开始使用 Amphitheatre，并且这意味着我们需要 `amp`，这是用于管理
Amphitheatre 上应用程序的 CLI 应用程序。如果您已经安装了它，请继续。如果没有，请
跳转到 [我们的安装指南](@/installation/_index.zh.md)。

## 在 .amp.toml 中

`.amp.toml` 文件现在包含用于部署您的 `Character` 的默认配置。如果我们查看
`.amp.toml` 文件，可以在其中看到：

```toml
name = "amp-example-sway"
version = "0.1.0"
edition = "v1"
description = "A simple Sway example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-sway"
repository = "https://github.com/amphitheatre-app/amp-example-sway"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "sway", "getting-started"]
categories = ["example"]

[build]
builder = "ghcr.io/amp-buildpacks/sway-builder"

[partners]
sway = { version = "stable", registry = "catalog" }

[build.env]
BP_ENABLE_FORC_DEPLOY = "true"
BP_HOME = "/layers/amp-buildpacks_sway/forc-amd64/fuel"
```

如果该文件存在，amp 命令将始终引用当前目录中的此文件，特别是开始时的 Character
名称值。该名称将用于标识 Character 到 Amphitheatre 平台。文件的其余部分包含部署
Character 时要应用的设置。

有关更多选项，请参阅 [sway-builder 文档](https://github.com/amp-buildpacks/sway-builder)。

## 部署到 Amphitheatre

要部署您的 Character，请运行：

```sh
amp run
```

这将查找我们的 `.amp.toml` 文件，并从中获取 Character 名称 `amp-example-sway`。然
后，`amp` 将开始部署 Character 到 Amphitheatre 平台。`amp` 将在完成时返回到命令
