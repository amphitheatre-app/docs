+++
title = "amp apply"
weight = 1
+++

Apply hydrated manifests to a cluster

## Usage
```
amp apply [options]
```

## Options

```
    --assume-yes=false: If true, amp will skip yes/no confirmation from the user and default to yes
-c, --config='': File for global configurations (defaults to $HOME/.amp/config)
-f, --filename='.amp.toml': Path or URL to the Amphitheatre config file
    --force=false: Recreate Kubernetes resources if necessary for deployment, warning: might cause downtime!
    --iterative-status-check=false: Run `status-check` iteratively after each deploy step, instead of all-together at the end of all deploys (default).
    --kube-context='': Deploy to this Kubernetes context
    --kubeconfig='': Path to the kubeconfig file to use for CLI requests.
-m, --module=[]: Filter Amphitheatre configs to only the provided named modules
-n, --namespace='': Run deployments in the specified namespace
-p, --profile=[]: Activate profiles by name (prefixed with `-` to disable a profile)
    --remote-cache-dir='': Specify the location of the git repositories cache (default $HOME/.amp/repos)
    --status-check=: Wait for deployed resources to stabilize
    --sync-remote-cache='always': Controls how Amphitheatre manages the remote config cache (see `remote-cache-dir`). One of `always` (default), `missing`, or `never`. `always` syncs remote repositories to latest on access. `missing` only clones remote repositories if they do not exist locally. `never` means the user takes responsibility for updating remote repositories.
    --tail=false: Stream logs from deployed objects
    --wait-for-connection=false: Blocks ending execution of amp until the /v2/events gRPC/HTTP endpoint is hit
```      

> Use "amp options" for a list of global command-line options (applies to all commands).

## Examples

#### Hydrate Kubernetes pod manifest first
```
amp render --output rendered-pod.yaml
```

#### Then create resources on your cluster from that hydrated manifest
```
amp apply rendered-pod.yaml
```

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_CONFIG` (same as `--config`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_FORCE` (same as `--force`)
* `AMP_ITERATIVE_STATUS_CHECK` (same as `--iterative-status-check`)
* `AMP_KUBE_CONTEXT` (same as `--kube-context`)
* `AMP_KUBECONFIG` (same as `--kubeconfig`)
* `AMP_MODULE` (same as `--module`)
* `AMP_NAMESPACE` (same as `--namespace`)
* `AMP_PROFILE` (same as `--profile`)
* `AMP_REMOTE_CACHE_DIR` (same as `--remote-cache-dir`)
* `AMP_STATUS_CHECK` (same as `--status-check`)
* `AMP_SYNC_REMOTE_CACHE` (same as `--sync-remote-cache`)
* `AMP_TAIL` (same as `--tail`)
* `AMP_WAIT_FOR_CONNECTION` (same as `--wait-for-connection`)