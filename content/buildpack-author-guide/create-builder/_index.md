+++
title = "Create a Builder"
description = "Conveniently build a Builder project through Packer with almost no need to modify the code."
weight = 4
sort_by = "weight"

[extra.sidebar]
linkable = true
+++

This article will use the `Leo` smart contract as an example to introduce how to create a `Builder`.
To comply with the Buildpacks specification, creating a `Builder` requires you to write a `builder.toml` file listing the names of multiple other `buildpack`s.

## Relationship between Builder, Meta-Buildpack, and Buildpack

To better understand the hierarchical relationship between builder, meta-buildpack, and buildpack, let's continue to analyze with the example of "Leo".

• At the builder level, we refer to leo-builder, which only contains meta-buildpack.

• Meta-buildpack is a more comprehensive concept, which includes multiple underlying packages and a procfile. Taking Leo as an example, it covers leo-dist buildpack and procfile buildpack.

• Buildpack constitutes the lowest level implementation of the entire architecture, distributed across multiple repositories. Taking leo-dist buildpack as an example, they respectively include the implementation of business logic, which are basically functions that all chains need to implement, such as wallet initialization, fetch devnet faucet, contract compilation, contract deployment, and other important operations. These operations often require a significant amount of time to complete, reflecting the understanding and familiarity with smart contracts and blockchain knowledge.

## Overall Process

Here are the steps to initialize a `Builder`:

1. Create a `Builder` configuration file named `config.toml`.

2. In the `config.toml` file, specify the dependencies of the `Meta Buildpack` you wish to combine, for example:

```toml
[[dependencies]]
repo = "amp-buildpacks/leo"
version = "0.1.7"
```

3. Use the `Packer CLI` tool to initialize your `Builder` project.

4. Run the official `pack builder` command to build your `Builder`.

## Initializing the Project

### 1. Directly create the project and corresponding folders

```bash
packer init -t builder -c config.toml leo-builder
```

### 2. Use in an empty folder

First, go into an empty folder, then run:

```bash
cd leo-builder
packer init -t builder -c config.toml
```

### 3. Force overwrite an existing project

If you wish to overwrite an existing project, you can use the following command:

```bash
packer init -t builder -f -c config.toml leo-builder
```

> Please note that using the `-f` option will forcefully overwrite the existing project and all its contents. Before performing this operation, make sure you have fully backed up the project to prevent any unexpected issues.

> This will complete the creation of the `leo-builder` project.

Check the project file tree, and you will find the following related files have been automatically created:

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