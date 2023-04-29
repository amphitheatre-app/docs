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
  -v, --verbose...             More output per occurrence
  -q, --quiet...               Less output per occurrence
  -c, --config <CONFIG>        File for global configurations [env: AMP_CONFIG=] [default: ~/.config/amphitheatre/config.toml]
      --interactive            Allow user prompts for more information [env: AMP_INTERACTIVE=]
      --timestamps             Print timestamps in logs [env: AMP_TIMESTAMPS=]
      --update-check           Check for a more recent version of Amphitheatre [env: AMP_UPDATE_CHECK=]
      --verbosity <VERBOSITY>  Log level: one of [panic fatal error warning info debug trace] [env: AMP_VERBOSITY=] [default: warning]
  -h, --help                   Print help
```

## Environment vars

* `AMP_CONFIG` (same as `--config`)
* `AMP_INTERACTIVE` (same as `--interactive`)
* `AMP_TIMESTAMPS` (same as `--timestamps`)
* `AMP_UPDATE_CHECK` (same as `--update-check`)
* `AMP_VERBOSITY` (same as `--verbosity`)
