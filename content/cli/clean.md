+++
title = "amp clean"
weight = 1
+++

Delete any resources deployed by Amphitheatre

## Usage
```
amp clean [options]
``` 

## Options
```
    --assume-yes=false: If true, amp will skip yes/no confirmation from the user and default to yes
-c, --config='': File for global configurations (defaults to $HOME/.amp/config)
-d, --default-repo='': Default repository value (overrides global config)
    --detect-minikube=true: Use heuristics to detect a minikube cluster
    --dry-run=false: Don't delete resources, just print them.
-f, --filename='.amp.yaml': Path or URL to the Amphitheatre config file
    --kube-context='': Deploy to this Kubernetes context
    --kubeconfig='': Path to the kubeconfig file to use for CLI requests.
-m, --module=[]: Filter Amphitheatre configs to only the provided named modules
-n, --namespace='': Run deployments in the specified namespace
-p, --profile=[]: Activate profiles by name (prefixed with `-` to disable a profile)
    --profile-auto-activation=true: Set to false to disable profile auto activation
    --propagate-profiles=true: Setting '--propagate-profiles=false' disables propagating profiles set by the '--profile' flag across config dependencies. This mean that only profiles defined directly in the target '.amp.yaml' file are activated.
    --remote-cache-dir='': Specify the location of the git repositories cache (default $HOME/.amp/repos)
    --sync-remote-cache='always': Controls how Amphitheatre manages the remote config cache (see `remote-cache-dir`). One of `always` (default), `missing`, or `never`. `always` syncs remote repositories to latest on access. `missing` only clones remote repositories if they do not exist locally. `never` means the user takes responsibility for updating remote repositories.
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## Examples
#### Print the resources to be deleted
```
amp clean --dry-run
```

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_CONFIG` (same as `--config`)
* `AMP_DEFAULT_REPO` (same as `--default-repo`)
* `AMP_DETECT_MINIKUBE` (same as `--detect-minikube`)
* `AMP_DRY_RUN` (same as `--dry-run`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_KUBE_CONTEXT` (same as `--kube-context`)
* `AMP_KUBECONFIG` (same as `--kubeconfig`)
* `AMP_MODULE` (same as `--module`)
* `AMP_NAMESPACE` (same as `--namespace`)
* `AMP_PROFILE` (same as `--profile`)
* `AMP_PROFILE_AUTO_ACTIVATION` (same as `--profile-auto-activation`)
* `AMP_PROPAGATE_PROFILES` (same as `--propagate-profiles`)
* `AMP_REMOTE_CACHE_DIR` (same as `--remote-cache-dir`)
* `AMP_SYNC_REMOTE_CACHE` (same as `--sync-remote-cache`)
