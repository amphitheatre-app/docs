+++
title = "amp run"
description = "Run a pipeline, build & deploy once"
weight = 1
+++

## Usage
```
amp run [options]
```

## Options

```
    --assume-yes <ASSUME_YES>  If true, amp will skip yes/no confirmation from the user [default: true] [possible values: true, false]
    --auto-create-config       If true, amp will try to create a config for the user's run if it doesn't find one
    --cleanup                  Show the build logs and output
-f, --filename <FILENAME>      Path or URL to the Amphitheatre config file [default: .amp.toml]
    --force                    Recreate Kubernetes resources if necessary for deployment, warning: might cause downtime!
    --iterative-status-check   Run `status-check` iteratively after each deploy step, instead of all-together at the end of all deploys (default)
-p, --profile <PROFILE>        Activate profiles by name (prefixed with `-` to disable a profile) [default: []]
    --status-check             Wait for deployed resources to stabilize
    --tail                     Stream logs from deployed objects
```


> Use "amp options" for a list of global command-line options (applies to all commands).

## Examples

#### Build, test, deploy and tail the logs
```
amp run --tail
```

#### Run with a given profile
```
amp run -p <profile>
```

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_AUTO_CREATE_CONFIG` (same as `--auto-create-config`)
* `AMP_CLEANUP` (same as `--cleanup`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_FORCE` (same as `--force`)
* `AMP_ITERATIVE_STATUS_CHECK` (same as `--iterative-status-check`)
* `AMP_PROFILE` (same as `--profile`)
* `AMP_STATUS_CHECK` (same as `--status-check`)
* `AMP_TAIL` (same as `--tail`)
