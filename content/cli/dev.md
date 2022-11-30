+++
title = "amp dev"
description = "Run a pipeline in development mode"
weight = 1
+++

## Usage
```
amp dev [options]
```

## Options

```
    --assume-yes <ASSUME_YES>  If true, amp will skip yes/no confirmation from the user [default: true] [possible values: true, false]
    --auto-build               When set to false, builds wait for API request instead of running automatically
    --auto-create-config       If true, amp will try to create a config for the user's run if it doesn't find one
    --auto-deploy              When set to false, deploys wait for API request instead of running automatically
    --auto-sync                When set to false, syncs wait for API request instead of running automatically
    --cleanup                  Delete deployments after dev or debug mode is interrupted
-c, --config <CONFIG>          File for global configurations [default: ~/.amp/config]
-f, --filename <FILENAME>      Path or URL to the Amphitheatre config file [default: .amp.toml]
    --force                    Recreate Kubernetes resources if necessary for deployment, warning: might cause downtime!
    --iterative-status-check   Run `status-check` iteratively after each deploy step, instead of all-together at the end of all deploys (default)
-p, --profile <PROFILE>        Activate profiles by name (prefixed with `-` to disable a profile) [default: []]
    --status-check             Wait for deployed resources to stabilize
    --tail                     Stream logs from deployed objects
    --trigger <TRIGGER>        How is change detection triggered? (polling, notify, or manual) [default: notify]
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_AUTO_BUILD` (same as `--auto-build`)
* `AMP_AUTO_CREATE_CONFIG` (same as `--auto-create-config`)
* `AMP_AUTO_DEPLOY` (same as `--auto-deploy`)
* `AMP_AUTO_SYNC` (same as `--auto-sync`)
* `AMP_CLEANUP` (same as `--cleanup`)
* `AMP_CONFIG` (same as `--config`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_FORCE` (same as `--force`)
* `AMP_ITERATIVE_STATUS_CHECK` (same as `--iterative-status-check`)
* `AMP_PROFILE` (same as `--profile`)
* `AMP_STATUS_CHECK` (same as `--status-check`)
* `AMP_TAIL` (same as `--tail`)
* `AMP_TRIGGER` (same as `--trigger`)
