+++
title = "amp config"
weight = 1
+++

Interact with the global Amphitheatre config file (defaults to `$HOME/.amp/config`)

## Available Commands
- [list](#amp-config-list)        List all values set in the global Amphitheatre config
- [set](#amp-config-set)         Set a value in the global Amphitheatre config
- [unset](#amp-config-unset)       Unset a value in the global Amphitheatre config

> Use "amp <command> --help" for more information about a given command.

## amp config list

List all values set in the global Amphitheatre config


### Usage
```
amp config list [options]
```

### Options

```
-a, --all=false: Show values for all kubecontexts
-c, --config='': Path to Amphitheatre config
-k, --kube-context='': Kubectl context to set values against
```

> Use "amp options" for a list of global command-line options (applies to all commands).

### Environment vars

* `AMP_ALL` (same as `--all`)
* `AMP_CONFIG` (same as `--config`)
* `AMP_KUBE_CONTEXT` (same as `--kube-context`)

## amp config set

Set a value in the global Amphitheatre config

### Usage
```
amp config set [options]
```

### Options
```
-c, --config='': Path to Amphitheatre config
-g, --global=false: Set value for global config
-k, --kube-context='': Kubectl context to set values against
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

#### Disable pushing images for a given Kubernetes context
```
amp config set --kube-context <mycluster> local-cluster true
```

### Environment vars

* `AMP_CONFIG` (same as `--config`)
* `AMP_GLOBAL` (same as `--global`)
* `AMP_KUBE_CONTEXT` (same as `--kube-context`)

## amp config unset

Unset a value in the global Amphitheatre config

### Usage
```
amp config unset [options]
```

### Options
```
-c, --config='': Path to Amphitheatre config
-g, --global=false: Set value for global config
-k, --kube-context='': Kubectl context to set values against
```

> Use "amp options" for a list of global command-line options (applies to all commands).

### Environment vars

* `AMP_CONFIG` (same as `--config`)
* `AMP_GLOBAL` (same as `--global`)
* `AMP_KUBE_CONTEXT` (same as `--kube-context`)