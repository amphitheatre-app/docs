+++
title = "amp diagnose"
description = "Run a diagnostic on Amphitheatre"
weight = 1
+++


## Usage
```
amp diagnose [options]
```

## Option
```
    --assume-yes <ASSUME_YES>  If true, amp will skip yes/no confirmation from the user [default: true] [possible values: true, false]
-c, --config <CONFIG>          File for global configurations [default: ~/.amp/config]
-f, --filename <FILENAME>      Path or URL to the Amphitheatre config file [default: .amp.toml]
-p, --profile <PROFILE>        Activate profiles by name (prefixed with `-` to disable a profile) [default: []]
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## Examples
#### Search for configuration issues and print the effective configuration
```
amp diagnose
```

#### Print the configuration for given profile
```
amp diagnose --profile PROFILE
```

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_CONFIG` (same as `--config`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_PROFILE` (same as `--profile`)
