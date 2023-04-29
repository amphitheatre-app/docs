+++
title = "amp init"
description = "Create a new Amphitheatre character in an existing directory"
weight = 1
+++


## Usage
```
amp init [options]
```

## Options
```
    --assume-yes <ASSUME_YES>  If true, amp will skip yes/no confirmation from the user [default: true] [possible values: true, false]
-f, --filename <FILENAME>      File to write generated manifests to [default: .amp.toml]
    --force                    Force the generation of the Amphitheatre character
    --name <NAME>              Set the character name. Defaults to the directory name
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_FORCE` (same as `--force`)
* `AMP_NAME` (same as `--name`)
