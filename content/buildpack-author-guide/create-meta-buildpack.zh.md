+++
title = "创建 Meta Buildpack"
description = "通过 Packer 便捷地构建 Meta Buildpack 项目，几乎不需要对代码进行任何修改。"
weight = 3
sort_by = "weight"

[extra.sidebar]
linkable = true
+++

本文将以 `Leo` 智能合约为例，介绍如何创建一个 `Meta Buildpack`。

`Meta Buildpack` 是一种特殊的 `buildpack`，它能够将其他 `buildpack` 集合在一起。

要遵循 Buildpacks 规范，创建一个 `Meta Buildpack` 需要您编写一个 `buildpack.toml` 文件，其中列出了多个其他 `buildpack` 的名称。

## 整体流程

以下是初始化 `Meta Buildpack` 的步骤：

1. 创建一个名为 `config.toml` 的 `Meta Buildpack` 配置文件。

2. 在 `config.toml` 文件中，指定您希望组合的 `buildpack` 依赖项，例如：

```toml
[[dependencies]]
repo = "amp-buildpacks/leo-dist"
version = "0.1.6"

[[dependencies]]
repo = "amp-buildpacks/aleo"
version = "0.1.10"
```

3. 利用 `Packer CLI` 工具来初始化您的 `Meta Buildpack` 项目。

4. 运行官方 `pack build` 命令来构建您的 `Meta Buildpack`。

## 初始化项目

### 1. 直接创建项目及对应文件夹

```bash
packer init -t meta -c config.toml leo
```

### 2. 在空文件夹中使用

首先，进入一个空的文件夹，然后运行：

```bash
cd leo
packer init -t meta -c config.toml
```

### 3. 强制覆盖已存在的项目

如果您希望覆盖一个已经存在的项目，可以使用以下命令：

```bash
packer init -t meta -f -c config.toml leo
```

> 请注意，使用 `-f` 选项将强制覆盖现有的项目及其中所有文件内容。在执行此操作之前，请确保您已经对项目进行了完整备份，以防不测。

> 至此，`leo Meta Buildpack` 项目创建完成。

查看项目文件树，您将会发现以下相关文件已自动创建。

```bash
> tree .github .
.github
├── pipeline-descriptor.yml
└── workflows
    └── pb-update-pipeline.yml
.
├── LICENSE
├── README.md
├── buildpack.toml
└── package.toml
```

