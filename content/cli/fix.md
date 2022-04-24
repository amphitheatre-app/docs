+++
title = "amp fix"
weight = 1
+++

Update old configuration to a newer schema version

## Usage
```
amp fix [options]
```

## Options
```
    --assume-yes=false: If true, amp will skip yes/no confirmation from the user and default to yes
-f, --filename='.amp.yaml': Path or URL to the Amphitheatre config file
-m, --module=[]: Filter Amphitheatre configs to only the provided named modules
-o, --output='': File to write the changed config (instead of standard output)
    --overwrite=false: Overwrite original config with fixed config
    --remote-cache-dir='': Specify the location of the git repositories cache (default $HOME/.amp/repos)
    --sync-remote-cache='missing': Controls how Amphitheatre manages the remote config cache (see `remote-cache-dir`). One of `always` (default), `missing`, or `never`. `always` syncs remote repositories to latest on access. `missing` only clones remote repositories if they do not exist locally. `never` means the user takes responsibility for updating remote repositories.
    --version='amp/v3alpha1': Target schema version to upgrade to
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## Examples

#### Update ".amp.yaml" in the current folder to the latest version
```
amp fix
```

#### Update ".amp.yaml" in the current folder to version "amp/v1"
```
amp fix --version amp/v1
```

#### Update ".amp.yaml" in the current folder in-place
```
amp fix --overwrite
```

#### Update ".amp.yaml" and write the output to a new file
```
amp fix --output .amp.new.yaml
```

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_MODULE` (same as `--module`)
* `AMP_OUTPUT` (same as `--output`)
* `AMP_OVERWRITE` (same as `--overwrite`)
* `AMP_REMOTE_CACHE_DIR` (same as `--remote-cache-dir`)
* `AMP_SYNC_REMOTE_CACHE` (same as `--sync-remote-cache`)
* `AMP_VERSION` (same as `--version`)
