+++
title = "amp render"
description = "Perform all image builds, and output rendered Kubernetes manifests"
weight = 1
+++


## Usage
```
amp render [options]
```

## Options

```
    --assume-yes <ASSUME_YES>  If true, amp will skip yes/no confirmation from the user [default: true] [possible values: true, false]
-f, --filename <FILENAME>      Path or URL to the Amphitheatre config file [default: .amp.toml]
    --loud                     Show the build logs and output
-o, --output <OUTPUT>          File to write the changed config (instead of standard output)
-p, --profile <PROFILE>        Activate profiles by name (prefixed with `-` to disable a profile) [default: []]
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_LOUD` (same as `--loud`)
* `AMP_OUTPUT` (same as `--output`)
* `AMP_PROFILE` (same as `--profile`)
