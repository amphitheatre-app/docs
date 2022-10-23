+++
title = "CLI Manual"
weight = 3
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

* [amp build](@/cli/build.md) - to just build and tag your image(s)
* [amp deploy](@/cli/deploy.md) - to deploy the given image(s)
* [amp clean](@/cli/clean.md) - to cleanup the deployed artifacts
* [amp render](@/cli/render.md) - build and tag images, and output templated
  Kubernetes manifests
* [amp apply](@/cli/apply.md) - to apply hydrated manifests to a cluster

Getting started with a new project:

* [amp init](@/cli/init.md) - to bootstrap Amphitheatre config
* [amp fix](@/cli/fix.md) - to upgrade from older .amp.toml schema version to newer
  .amp.toml schema version 

Other Commands:

* amp help - print help
* [amp version](@/cli/version.md) - get Amphitheatre version
* [amp completion](@/cli/completion.md) - setup tab completion for the CLI
* [amp config](@/cli/config.md) - manage context specific parameters
* amp credits - export third party notices to given path (./amp-credits by
  default)
* [amp diagnose](@/cli/diagnose.md) - diagnostics of Amphitheatre works in your
  project
* [amp schema](@/cli/schema.md) - list and print json schemas used to validate
  .amp.toml configuration

## Global flags

* `-h, --help`: Prints the HELP file for the current command.
* `-v, --verbosity LOG-LEVEL`: Uses a specific log level. Available log levels are `info`, `warn`, `error`, `fatal` `debug` and `trace`. Default value is `warn`.


## Global environment variables

* `AMP_UPDATE_CHECK` Enables checking for latest version of the Amphitheatre binary. By default it's `true`. 

## Environment vars

* `AMP_COLOR` (same as `--color`)
* `AMP_INTERACTIVE` (same as `--interactive`)
* `AMP_TIMESTAMPS` (same as `--timestamps`)
* `AMP_UPDATE_CHECK` (same as `--update-check`)
* `AMP_VERBOSITY` (same as `--verbosity`)
