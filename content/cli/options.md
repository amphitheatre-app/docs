+++
title = "amp options"
description = "List of global command-line options (applies to all commands)"
weight = 1
+++

## Usage
```
amp options
```

## Options
The following options can be passed to any command:

```
-h, --help          Print help information
    --interactive   Allow user prompts for more information
-q, --quiet...      Less output per occurrence
    --timestamps    Print timestamps in logs
    --update-check  Check for a more recent version of Amphitheatre
-v, --verbose...    More output per occurrence
```

## Environment vars

* `AMP_INTERACTIVE` (same as `--interactive`)
* `AMP_TIMESTAMPS` (same as `--timestamps`)
* `AMP_UPDATE_CHECK` (same as `--update-check`)
* `AMP_VERBOSITY` (same as `--verbosity`)
