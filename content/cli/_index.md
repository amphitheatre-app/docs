+++
title = "CLI Manual"
weight = 60
sort_by = "weight"

[extra]
listing = false

[extra.sidebar]
linkable = true
+++

Amphitheatre command-line interface provides the following commands:


End-to-end pipelines:

* [amp run](@/cli/run.md) - to build & deploy once
* [amp dev](@/cli/dev.md) - to trigger the watch loop build & deploy workflow with
  cleanup on exit
* [amp debug](@/cli/debug.md) - to run a pipeline in debug mode

Pipeline building blocks for CI/CD:

* [amp deploy](@/cli/deploy.md) - to deploy the given image(s)
* [amp clean](@/cli/clean.md) - to cleanup the deployed artifacts
* [amp render](@/cli/render.md) - build and tag images, and output templated
  Kubernetes manifests

Getting started with a new project:

* [amp init](@/cli/init.md) - to bootstrap Amphitheatre config

Other Commands:

* amp help - print help
* [amp version](@/cli/version.md) - get Amphitheatre version
* [amp completion](@/cli/completion.md) - setup tab completion for the CLI
* [amp config](@/cli/config.md) - interact with the global config file
* [amp context](@/cli/context.md) - configure access to multiple clusters
* [amp diagnose](@/cli/diagnose.md) - diagnostics of Amphitheatre works in your project

## Global flags

* `-v, --verbose...`         More output per occurrence
* `-q, --quiet...`           Less output per occurrence
* `-h, --help`               Print help information


## Global environment variables

* `AMP_UPDATE_CHECK` Enables checking for latest version of the Amphitheatre binary. By default it's `true`.

## Environment vars

* `AMP_INTERACTIVE` (same as `--interactive`)
* `AMP_TIMESTAMPS` (same as `--timestamps`)
* `AMP_UPDATE_CHECK` (same as `--update-check`)
* `AMP_VERBOSITY` (same as `--verbosity`)
