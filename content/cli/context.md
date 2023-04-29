+++
title = "amp context"
description = "Configure access to multiple clusters."
weight = 1
+++

A context is a set of cluster access parameters. Each context contains its name and
server address and access credentials, etc. The current context is the default value
for any Amphitheatre CLI command.

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

> Use "amp options" for a list of global command-line options (applies to all
> commands).

### Example

```
amp context show
```

```
Context {
    name: "default",
    url: "http://localhost:8170",
    token: "",
}
```

## amp context list

List all available contexts

### Usage
```
amp context list [options]
```

> Use "amp options" for a list of global command-line options (applies to all commands).

### Example

```
amp context list
```

```
Context {
    name: "default",
    url: "http://localhost:8170",
    token: "",
}
Context {
    name: "local",
    url: "http://api.amphitheatre.local:8170",
    token: "",
}
```

## amp context use

Select one of your existing contexts or to create a new one

### Usage
```
amp context use [OPTIONS] [NAME]
```

> Use "amp options" for a list of global command-line options (applies to all commands).

### Examples

#### Set the default context.

```
amp context use
```

This will prompt you to select one of your existing contexts or to create a new one.

#### You can also specify a name:
```
amp context use local
```

## amp context delete

Delete a context

### Usage
```
amp context delete [OPTIONS] <NAME>
```

> Use "amp options" for a list of global command-line options (applies to all commands).

### Examples

#### Delete the `local` context

```
amp context delete local
```
