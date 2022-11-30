+++
title = "amp deploy"
description = "Deploy pre-built artifacts"
weight = 1
+++

## Usage
```
amp deploy [options]
```

## Options

```
    --assume-yes <ASSUME_YES>  If true, amp will skip yes/no confirmation from the user [default: true] [possible values: true, false]
-c, --config <CONFIG>          File for global configurations [default: ~/.amp/config]
-f, --filename <FILENAME>      Path or URL to the Amphitheatre config file [default: .amp.toml]
    --force                    Recreate Kubernetes resources if necessary for deployment, warning: might cause downtime!
    --iterative-status-check   Run `status-check` iteratively after each deploy step, instead of all-together at the end of all deploys (default)
-p, --profile <PROFILE>        Activate profiles by name (prefixed with `-` to disable a profile) [default: []]
    --skip-render              Don't render the manifests, just deploy them
    --status-check             Wait for deployed resources to stabilize
    --tail                     Stream logs from deployed objects
```

> Use "amp options" for a list of global command-line options (applies to all commands).


## Examples

#### Deploy without first rendering the manifests
```
amp deploy --skip-render
```

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_CONFIG` (same as `--config`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_FORCE` (same as `--force`)
* `AMP_ITERATIVE_STATUS_CHECK` (same as `--iterative-status-check`)
* `AMP_PROFILE` (same as `--profile`)
* `AMP_SKIP_RENDER` (same as `--skip-render`)
* `AMP_STATUS_CHECK` (same as `--status-check`)
* `AMP_TAIL` (same as `--tail`)
