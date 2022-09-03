+++
title = "amp test"
weight = 1
+++

Run tests against your built application images

## Usage
```
amp test [options]
```

## Options

```
    --assume-yes=false: If true, amp will skip yes/no confirmation from the user and default to yes
-a, --build-artifacts=: File containing build result from a previous 'amp build --file-output'
-c, --config='': File for global configurations (defaults to $HOME/.amp/config)
-f, --filename='.amp.toml': Path or URL to the Amphitheatre config file
-i, --images=: A list of pre-built images to deploy, either tagged images or NAME=TAG pairs
-m, --module=[]: Filter Amphitheatre configs to only the provided named modules
-p, --profile=[]: Activate profiles by name (prefixed with `-` to disable a profile)
    --profile-auto-activation=true: Set to false to disable profile auto activation
    --propagate-profiles=true: Setting '--propagate-profiles=false' disables propagating profiles set by the '--profile' flag across config dependencies. This mean that only profiles defined directly in the target '.amp.toml' file are activated.
    --remote-cache-dir='': Specify the location of the git repositories cache (default $HOME/.amp/repos)
    --rpc-http-port=: tcp port to expose the Amphitheatre API over HTTP REST
    --rpc-port=: tcp port to expose the Amphitheatre API over gRPC
    --sync-remote-cache='always': Controls how Amphitheatre manages the remote config cache (see `remote-cache-dir`). One of `always` (default), `missing`, or `never`. `always` syncs remote repositories to latest on access. `missing` only clones remote repositories if they do not exist locally. `never` means the user takes responsibility for updating remote repositories.
    --wait-for-connection=false: Blocks ending execution of amp until the /v2/events gRPC/HTTP endpoint is hit
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## Examples
#### Build the artifacts and collect the tags into a file
```
amp build --file-output=tags.json
```

#### Run test against images previously built by Amphitheatre into a 'tags.json' file
```
amp test --build-artifacts=tags.json
```

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_BUILD_ARTIFACTS` (same as `--build-artifacts`)
* `AMP_CONFIG` (same as `--config`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_IMAGES` (same as `--images`)
* `AMP_MODULE` (same as `--module`)
* `AMP_PROFILE` (same as `--profile`)
* `AMP_PROFILE_AUTO_ACTIVATION` (same as `--profile-auto-activation`)
* `AMP_PROPAGATE_PROFILES` (same as `--propagate-profiles`)
* `AMP_REMOTE_CACHE_DIR` (same as `--remote-cache-dir`)
* `AMP_RPC_HTTP_PORT` (same as `--rpc-http-port`)
* `AMP_RPC_PORT` (same as `--rpc-port`)
* `AMP_SYNC_REMOTE_CACHE` (same as `--sync-remote-cache`)
* `AMP_WAIT_FOR_CONNECTION` (same as `--wait-for-connection`)