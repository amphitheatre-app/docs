+++
title = "amp clean"
weight = 1
+++

Delete any resources deployed by Amphitheatre

## Usage
```
amp clean [options]
```

## Options
```
    --assume-yes=false: If true, amp will skip yes/no confirmation from the user and default to yes
-c, --config='': File for global configurations (defaults to $HOME/.amp/config)
    --dry-run=false: Don't delete resources, just print them.
-f, --filename='.amp.toml': Path or URL to the Amphitheatre config file
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
