+++
title = "amp test"
description = "Run tests against your built application images"
weight = 1
+++

## Usage
```
amp test [options]
```

## Options

```
    --assume-yes <ASSUME_YES>  If true, amp will skip yes/no confirmation from the user [default: true] [possible values: true, false]
-f, --filename <FILENAME>      Path or URL to the Amphitheatre config file [default: .amp.toml]
-p, --profile <PROFILE>        Activate profiles by name (prefixed with `-` to disable a profile) [default: []]
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_PROFILE` (same as `--profile`)
