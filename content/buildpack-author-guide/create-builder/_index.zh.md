+++
title = "创建 Builder"
description = "通过 Packer 便捷地构建 Builder 项目，几乎不需要对代码进行任何修改。"
weight = 4
sort_by = "weight"

[extra.sidebar]
linkable = true
+++

本文将以 `Leo` 智能合约为例，介绍如何创建一个 `Builder`。

要遵循 Buildpacks 规范，创建一个 `Builder` 需要您编写一个 `builder.toml` 文件，其中列出了多个其他 `buildpack` 的名称。

## builder, meta-buildpack 和 buildpack 三者关系

为了更好地理解 `builder`、`meta-buildpack` 以及 `buildpack` 三者之间的层次关系，我们继续以"Leo"为例进行分析。

• 在 `builder` 层面，我们指的是 `leo-builder`，它仅包含 `meta-buildpack`。

• `meta-buildpack` 则是一个更为综合的概念，它包含了多个底层包以及 `procfile`。以 `leo` 为例，它涵盖了 `leo-dist buildpack` 以及 `procfile buildpack`。

• 而 `buildpack` 则构成了整个架构的最底层实现，它分布在多个仓库中。以 `leo-dist buildpack` 为例，它们分别包含了业务逻辑的实现，这是基本上所有链都需要实现的功能，比如钱包初始化、devnet取水、合约编译、合约部署等重要操作。这些操作往往需要花费较长时间来完成，这也体现了对智能合约以及区块链知识的理解和熟悉程度。

## 整体流程

以下是初始化 `Builder` 的步骤：

1. 创建一个名为 `config.toml` 的 `Builder` 配置文件。

2. 在 `config.toml` 文件中，指定您希望组合的 `Meta Buildpack` 依赖项，例如：

```toml
[[dependencies]]
repo = "amp-buildpacks/leo"
version = "0.1.7"
```

3. 利用 `Packer CLI` 工具来初始化您的 `Builder` 项目。

4. 运行官方 `pack builder` 命令来构建您的 `Builder`。

## 初始化项目

### 1. 直接创建项目及对应文件夹

```bash
packer init -t builder -c config.toml leo-builder
```

### 2. 在空文件夹中使用

首先，进入一个空的文件夹，然后运行：

```bash
cd leo-builder
packer init -t builder -c config.toml
```

### 3. 强制覆盖已存在的项目

如果您希望覆盖一个已经存在的项目，可以使用以下命令：

```bash
packer init -t builder -f -c config.toml leo-builder
```

> 请注意，使用 `-f` 选项将强制覆盖现有的项目及其中所有文件内容。在执行此操作之前，请确保您已经对项目进行了完整备份，以防不测。

> 至此，`leo-builder` 项目创建完成。

查看项目文件树，您将会发现以下相关文件已自动创建。

```bash
> tree .github .
.github
├── release-drafter.yml
└── workflows
    ├── pb-update-draft-release.yml
    └── push-image.yml
.
├── LICENSE
├── README.md
├── builder.toml
└── scripts
    └── smoke.sh
```

