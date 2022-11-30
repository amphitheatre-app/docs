+++
title = "amp context"
description = "Configure access to multiple clusters."
weight = 1
+++

A context is a group of cluster access parameters. Each context contains a
Kubernetes cluster, a user, and a namespace. The current context is the default
cluster/namespace for any Amphitheatre CLI command.

## Available Commands
- [show](#amp-context-show)     Print the current context
- [list](#amp-context-list)     List all available contexts
- [use](#amp-context-use)       Select one of your existing contexts or to create a new one
- [delete](#amp-context-delete) Delete a context

> Use "amp <command> --help" for more information about a given command.

## amp context show

Print the current context

### Usage
```
amp context show [options]
```

### Options

```
-c, --config <CONFIG>  File for global configurations [default: ~/.amp/config]
```

> Use "amp options" for a list of global command-line options (applies to all
> commands).

### Example

```
amp context show
```

```
{
  "name": "https://cloud.amphitheatre.app",
  "token": "REDACTED",
  "namespace": "default",
  "builder": "tcp://builder.cloud.amphitheatre.app:1234",
  "registry": "registry.cloud.amphitheatre.app",
}
```


## amp context list

List all available contexts

### Usage
```
amp context list [options]
```

### Options

```
-c, --config <CONFIG>  File for global configurations [default: ~/.amp/config]
```

> Use "amp options" for a list of global command-line options (applies to all commands).

### Example

```
amp context list
```

```
Name                              Namespace  Builder                                      Registry
https://cloud.amphitheatre.app *  default    tcp://builder.cloud.amphitheatre.app:1234    registry.cloud.amphitheatre.app
minikube                          default    docker                                       -
```

## amp context use

Select one of your existing contexts or to create a new one

### Usage
```
amp context use [OPTIONS] [URL]
```

### Options
```
-c, --config <CONFIG>  File for global configurations [default: ~/.amp/config]
```

> Use "amp options" for a list of global command-line options (applies to all commands).

### Examples

#### Set the default context.

```
amp context use
```

This will prompt you to select one of your existing contexts or to create a new one.

#### You can also specify an Amphitheatre URL:
```
amp context use https://cloud.amphitheatre.app
```

## amp context delete

Delete a context

### Usage
```
amp context delete [OPTIONS] <URL>
```

### Options
```
-c, --config <CONFIG>  File for global configurations [default: ~/.amp/config]
```

> Use "amp options" for a list of global command-line options (applies to all commands).

### Examples

#### Delete the Amphitheatre Cloud context

```
amp context delete https://cloud.amphitheatre.app
```
