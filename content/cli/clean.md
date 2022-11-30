+++
title = "amp clean"
description = "Delete any resources deployed by Amphitheatre"
weight = 1
+++

## Usage
```
amp clean [options]
```

## Options
```
    --assume-yes <ASSUME_YES>  If true, amp will skip yes/no confirmation from the user [default: true] [possible values: true, false]
-v, --verbose...               More output per occurrence
-c, --config <CONFIG>          File for global configurations (defaults to $HOME/.amp/config) [default: ~/.amp/config]
-q, --quiet...                 Less output per occurrence
    --dry-run                  If true, amp will skip yes/no confirmation from the user and default to yes
-f, --filename <FILENAME>      Path or URL to the Amphitheatre config file [default: .amp.toml]
-h, --help                     Print help information
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## Examples

#### Print the resources to be deleted
```
amp clean --dry-run
```

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_CONFIG` (same as `--config`)
* `AMP_DRY_RUN` (same as `--dry-run`)
* `AMP_FILENAME` (same as `--filename`)
