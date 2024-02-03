+++
title = "Create a Meta Buildpack"
description = "Conveniently build a Meta Buildpack project through Packer with almost no need to modify the code."
weight = 3
sort_by = "weight"

[extra.sidebar]
linkable = true
+++

This article will take the `Leo` smart contract as an example to introduce how to create a `Meta Buildpack`.

A `Meta Buildpack` is a special type of `buildpack` that can aggregate multiple other `buildpack` collections.

To comply with the Buildpacks specification, creating a `Meta Buildpack` requires you to write a `buildpack.toml` file listing the names of multiple other `buildpack`s.

## Overall Process

Here are the steps to initialize a `Meta Buildpack`:

1. Create a `Meta Buildpack` configuration file named `config.toml`.

2. In the `config.toml` file, specify the `buildpack` dependencies you wish to combine, for example:

```toml
[[dependencies]]
repo = "amp-buildpacks/leo-dist"
version = "0.1.6"

[[dependencies]]
repo = "amp-buildpacks/aleo"
version = "0.1.10"
```

3. Use the `Packer CLI` tool to initialize your `Meta Buildpack` project.

4. Run the official `pack build` command to build your `Meta Buildpack`.

## Initializing the Project

### 1. Create the project and corresponding folders directly

```bash
packer init -t meta -c config.toml leo
```

### 2. Use in an empty folder

First, go into an empty folder, then run:

```bash
cd leo
packer init -t meta -c config.toml
```

### 3. Force overwrite an existing project

If you wish to overwrite an existing project, you can use the following command:

```bash
packer init -t meta -f -c config.toml leo
```

> Please note that using the `-f` option will forcefully overwrite the existing project and all its contents. Before performing this operation, make sure you have fully backed up the project in case of any unforeseen issues.

> This will complete the creation of the `leo Meta Buildpack` project.

Check the project file tree, and you will find the following related files have been automatically created.
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
