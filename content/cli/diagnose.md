+++
title = "amp diagnose"
weight = 1
+++

Run a diagnostic on Amphitheatre

## Usage
```
amp diagnose [options]
```

## Option
```
    --assume-yes=false: If true, amp will skip yes/no confirmation from the user and default to yes
-c, --config='': File for global configurations (defaults to $HOME/.amp/config)
-f, --filename='.amp.toml': Path or URL to the Amphitheatre config file
-m, --module=[]: Filter Amphitheatre configs to only the provided named modules
-p, --profile=[]: Activate profiles by name (prefixed with `-` to disable a profile)
    --profile-auto-activation=true: Set to false to disable profile auto activation
    --propagate-profiles=true: Setting '--propagate-profiles=false' disables propagating profiles set by the '--profile' flag across config dependencies. This mean that only profiles defined directly in the target '.amp.toml' file are activated.
    --remote-cache-dir='': Specify the location of the git repositories cache (default $HOME/.amp/repos)
    --sync-remote-cache='missing': Controls how Amphitheatre manages the remote config cache (see `remote-cache-dir`). One of `always` (default), `missing`, or `never`. `always` syncs remote repositories to latest on access. `missing` only clones remote repositories if they do not exist locally. `never` means the user takes responsibility for updating remote repositories.
    --toml-only=false: Only prints the effective .amp.toml configuration
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## Examples
#### Search for configuration issues and print the effective configuration
```
amp diagnose
```

#### Print the effective .amp.toml configuration for given profile
```
amp diagnose --toml-only --profile PROFILE
```

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_CONFIG` (same as `--config`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_MODULE` (same as `--module`)
* `AMP_PROFILE` (same as `--profile`)
* `AMP_PROFILE_AUTO_ACTIVATION` (same as `--profile-auto-activation`)
* `AMP_PROPAGATE_PROFILES` (same as `--propagate-profiles`)
* `AMP_REMOTE_CACHE_DIR` (same as `--remote-cache-dir`)
* `AMP_SYNC_REMOTE_CACHE` (same as `--sync-remote-cache`)
* `AMP_TOML_ONLY` (same as `--toml-only`)