+++
title = "amp deploy"
weight = 1
+++

Deploy pre-built artifacts

## Usage
```
amp deploy [options]
```

## Options

```
    --assume-yes=false: If true, amp will skip yes/no confirmation from the user and default to yes
-a, --build-artifacts=: File containing build result from a previous 'amp build --file-output'
    --build-concurrency=-1: Number of concurrently running builds. Set to 0 to run all builds in parallel. Doesn't violate build order among dependencies.
-c, --config='': File for global configurations (defaults to $HOME/.amp/config)
-d, --default-repo='': Default repository value (overrides global config)
    --detect-minikube=true: Use heuristics to detect a minikube cluster
-f, --filename='.amp.yaml': Path or URL to the Amphitheatre config file
    --force=false: Recreate Kubernetes resources if necessary for deployment, warning: might cause downtime!
    --hydration-dir='.kpt-pipeline': The directory to where the (kpt) hydration takes place. Default to a hidden directory .kpt-pipeline.
-i, --images=: A list of pre-built images to deploy, either tagged images or NAME=TAG pairs
    --iterative-status-check=false: Run `status-check` iteratively after each deploy step, instead of all-together at the end of all deploys (default).
    --kube-context='': Deploy to this Kubernetes context
    --kubeconfig='': Path to the kubeconfig file to use for CLI requests.
-l, --label=[]: Add custom labels to deployed objects. Set multiple times for multiple labels
    --load-images=false: If true, amp will force load the container images into the local cluster.
-m, --module=[]: Filter Amphitheatre configs to only the provided named modules
    --mute-logs=[]: mute logs for specified stages in pipeline (build, deploy, status-check, none, all)
-n, --namespace='': Run deployments in the specified namespace
    --platform=[]: The platform to target for the build artifacts
    --port-forward=off: Port-forward exposes service ports and container ports within pods and other resources (off, user, services, debug, pods)
-p, --profile=[]: Activate profiles by name (prefixed with `-` to disable a profile)
    --profile-auto-activation=true: Set to false to disable profile auto activation
    --propagate-profiles=true: Setting '--propagate-profiles=false' disables propagating profiles set by the '--profile' flag across config dependencies. This mean that only profiles defined directly in the target '.amp.yaml' file are activated.
    --remote-cache-dir='': Specify the location of the git repositories cache (default $HOME/.amp/repos)
    --resource-selector-rules-file='': Path to JSON file specifying the deny list of yaml objects for amp to NOT transform with 'image' and 'label' field replacements.  NOTE: this list is additive to amp's default denylist and denylist has priority over allowlist
    --rpc-http-port=: tcp port to expose the Amphitheatre API over HTTP REST
    --rpc-port=: tcp port to expose the Amphitheatre API over gRPC
    --skip-render=false: Don't render the manifests, just deploy them
    --status-check=: Wait for deployed resources to stabilize
    --sync-remote-cache='always': Controls how Amphitheatre manages the remote config cache (see `remote-cache-dir`). One of `always` (default), `missing`, or `never`. `always` syncs remote repositories to latest on access. `missing` only clones remote repositories if they do not exist locally. `never` means the user takes responsibility for updating remote repositories.
-t, --tag='': The optional custom tag to use for images which overrides the current Tagger configuration
    --tail=false: Stream logs from deployed objects
    --toot=false: Emit a terminal beep after the deploy is complete
    --wait-for-connection=false: Blocks ending execution of amp until the /v2/events gRPC/HTTP endpoint is hit
    --wait-for-deletions=true: Wait for pending deletions to complete before a deployment
    --wait-for-deletions-delay=2s: Delay between two checks for pending deletions
    --wait-for-deletions-max=1m0s: Max duration to wait for pending deletions
```

> Use "amp options" for a list of global command-line options (applies to all commands).


## Examples

#### Build the artifacts and collect the tags into a file
```
amp build --file-output=tags.json
```

#### Deploy those tags
```
amp deploy --build-artifacts=tags.json
```

#### Build the artifacts and then deploy them
```
amp build -q | amp deploy --build-artifacts -
```

#### Deploy without first rendering the manifests
```
amp deploy --skip-render
```

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_BUILD_ARTIFACTS` (same as `--build-artifacts`)
* `AMP_BUILD_CONCURRENCY` (same as `--build-concurrency`)
* `AMP_CONFIG` (same as `--config`)
* `AMP_DEFAULT_REPO` (same as `--default-repo`)
* `AMP_DETECT_MINIKUBE` (same as `--detect-minikube`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_FORCE` (same as `--force`)
* `AMP_HYDRATION_DIR` (same as `--hydration-dir`)
* `AMP_IMAGES` (same as `--images`)
* `AMP_ITERATIVE_STATUS_CHECK` (same as `--iterative-status-check`)
* `AMP_KUBE_CONTEXT` (same as `--kube-context`)
* `AMP_KUBECONFIG` (same as `--kubeconfig`)
* `AMP_LABEL` (same as `--label`)
* `AMP_LOAD_IMAGES` (same as `--load-images`)
* `AMP_MODULE` (same as `--module`)
* `AMP_MUTE_LOGS` (same as `--mute-logs`)
* `AMP_NAMESPACE` (same as `--namespace`)
* `AMP_PLATFORM` (same as `--platform`)
* `AMP_PORT_FORWARD` (same as `--port-forward`)
* `AMP_PROFILE` (same as `--profile`)
* `AMP_PROFILE_AUTO_ACTIVATION` (same as `--profile-auto-activation`)
* `AMP_PROPAGATE_PROFILES` (same as `--propagate-profiles`)
* `AMP_REMOTE_CACHE_DIR` (same as `--remote-cache-dir`)
* `AMP_RESOURCE_SELECTOR_RULES_FILE` (same as
  `--resource-selector-rules-file`)
* `AMP_RPC_HTTP_PORT` (same as `--rpc-http-port`)
* `AMP_RPC_PORT` (same as `--rpc-port`)
* `AMP_SKIP_RENDER` (same as `--skip-render`)
* `AMP_STATUS_CHECK` (same as `--status-check`)
* `AMP_SYNC_REMOTE_CACHE` (same as `--sync-remote-cache`)
* `AMP_TAG` (same as `--tag`)
* `AMP_TAIL` (same as `--tail`)
* `AMP_TOOT` (same as `--toot`)
* `AMP_WAIT_FOR_CONNECTION` (same as `--wait-for-connection`)
* `AMP_WAIT_FOR_DELETIONS` (same as `--wait-for-deletions`)
* `AMP_WAIT_FOR_DELETIONS_DELAY` (same as `--wait-for-deletions-delay`)
* `AMP_WAIT_FOR_DELETIONS_MAX` (same as `--wait-for-deletions-max`)