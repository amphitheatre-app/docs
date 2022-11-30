+++
title = "amp config"
description = "Interact with the global Amphitheatre config file (defaults to `~/.amp/config`)"
weight = 1
+++



## Available Commands
- [find](#amp-config-find)      Locate the config file
- [list](#amp-config-list)      List all values set in the global Amphitheatre config
- [set](#amp-config-set)        Set a value in the global Amphitheatre config
- [unset](#amp-config-unset)    Unset a value in the global Amphitheatre config

> Use "amp <command> --help" for more information about a given command.

## amp config find

Locate the config file


### Usage
```
amp config find [options]
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## amp config list

List all values set in the global Amphitheatre config


### Usage
```
amp config list [options]
```

### Options

```
-a, --all              Show values for all configs
-c, --config <CONFIG>  Path to Amphitheatre config [default: $~/.amp/config]
```

> Use "amp options" for a list of global command-line options (applies to all commands).

### Environment vars

* `AMP_ALL` (same as `--all`)
* `AMP_CONFIG` (same as `--config`)

## amp config set

Set a value in the global Amphitheatre config

### Usage
```
amp config set [OPTIONS] <KEY> <VALUE>
```

### Options
```
  -c, --config <CONFIG>  Path to Amphitheatre config [default: ~/.amp/config]
  -g, --global           Set value for global config
```

> Use "amp options" for a list of global command-line options (applies to all commands).

### Examples:

#### Mark a registry as insecure
```
amp config set insecure-registries <insecure1.io>
```

#### Globally set the default image repository
```
amp config set default-repo <myrepo>
```

#### Globally set multi-level repo support
```
amp config set multi-level-repo true
```

### Environment vars

* `AMP_CONFIG` (same as `--config`)
* `AMP_GLOBAL` (same as `--global`)

## amp config unset

Unset a value in the global Amphitheatre config

### Usage
```
amp config unset [OPTIONS] <KEY>
```

### Options
```
-c, --config <CONFIG>  Path to Amphitheatre config [default: $HOME/.amp/config]
-g, --global           Set value for global config
```

> Use "amp options" for a list of global command-line options (applies to all commands).

### Environment vars

* `AMP_CONFIG` (same as `--config`)
* `AMP_GLOBAL` (same as `--global`)
