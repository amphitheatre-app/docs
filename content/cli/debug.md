+++
title = "amp debug"
weight = 1
+++

Run a pipeline in debug mode

## Usage
```
amp debug [options]
```

## Options
```
    --assume-yes=false: If true, amp will skip yes/no confirmation from the user and default to yes
    --auto-build=false: When set to false, builds wait for API request instead of running automatically
    --auto-create-config=true: If true, amp will try to create a config for the user's run if it doesn't find one
    --auto-deploy=false: When set to false, deploys wait for API request instead of running automatically
    --auto-sync=false: When set to false, syncs wait for API request instead of running automatically
    --build-concurrency=-1: Number of concurrently running builds. Set to 0 to run all builds in parallel. Doesn't violate build order among dependencies.
    --cache-artifacts=true: Set to false to disable default caching of artifacts
    --cache-file='': Specify the location of the cache file (default $HOME/.amp/cache)
    --cleanup=true: Delete deployments after dev or debug mode is interrupted
-c, --config='': File for global configurations (defaults to $HOME/.amp/config)
-d, --default-repo='': Default repository value (overrides global config)
    --detect-minikube=true: Use heuristics to detect a minikube cluster
-f, --filename='.amp.yaml': Path or URL to the Amphitheatre config file
    --force=false: Recreate Kubernetes resources if necessary for deployment, warning: might cause downtime!
    --hydration-dir='.kpt-pipeline': The directory to where the (kpt) hydration takes place. Default to a hidden directory .kpt-pipeline.
    --insecure-registry=[]: Target registries for built images which are not secure
    --iterative-status-check=false: Run `status-check` iteratively after each deploy step, instead of all-together at the end of all deploys (default).
    --kube-context='': Deploy to this Kubernetes context
    --kubeconfig='': Path to the kubeconfig file to use for CLI requests.
-l, --label=[]: Add custom labels to deployed objects. Set multiple times for multiple labels
-m, --module=[]: Filter Amphitheatre configs to only the provided named modules
    --mute-logs=[]: mute logs for specified stages in pipeline (build, deploy, status-check, none, all)
-n, --namespace='': Run deployments in the specified namespace
    --no-prune=false: Skip removing images and containers built by Amphitheatre
    --no-prune-children=false: Skip removing layers reused by Amphitheatre
    --platform=[]: The platform to target for the build artifacts
    --port-forward=user,debug: Port-forward exposes service ports and container ports within pods and other resources (off, user, services, debug, pods)
-p, --profile=[]: Activate profiles by name (prefixed with `-` to disable a profile)
    --profile-auto-activation=true: Set to false to disable profile auto activation
    --propagate-profiles=true: Setting '--propagate-profiles=false' disables propagating profiles set by the '--profile' flag across config dependencies. This mean that only profiles defined directly in the target '.amp.yaml' file are activated.
    --protocols=[]: Priority sorted order of debugger protocols to support.
    --remote-cache-dir='': Specify the location of the git repositories cache (default $HOME/.amp/repos)
    --resource-selector-rules-file='': Path to JSON file specifying the deny list of yaml objects for amp to NOT transform with 'image' and 'label' field replacements.  NOTE: this list is additive to amp's default denylist and denylist has priority over allowlist
    --rpc-http-port=: tcp port to expose the Amphitheatre API over HTTP REST
    --rpc-port=: tcp port to expose the Amphitheatre API over gRPC
    --skip-tests=false: Whether to skip the tests after building
    --status-check=: Wait for deployed resources to stabilize
    --sync-remote-cache='always': Controls how Amphitheatre manages the remote config cache (see `remote-cache-dir`). One of `always` (default), `missing`, or `never`. `always` syncs remote repositories to latest on access. `missing` only clones remote repositories if they do not exist locally. `never` means the user takes responsibility for updating remote repositories.
-t, --tag='': The optional custom tag to use for images which overrides the current Tagger configuration
    --tail=true: Stream logs from deployed objects
    --toot=false: Emit a terminal beep after the deploy is complete
    --trigger='notify': How is change detection triggered? (polling, notify, or manual)
    --wait-for-connection=false: Blocks ending execution of amp until the /v2/events gRPC/HTTP endpoint is hit
    --wait-for-deletions=true: Wait for pending deletions to complete before a deployment
    --wait-for-deletions-delay=2s: Delay between two checks for pending deletions
    --wait-for-deletions-max=1m0s: Max duration to wait for pending deletions
-w, --watch-image=[]: Choose which artifacts to watch. Artifacts with image names that contain the expression will be watched only. Default is to watch sources for all artifacts
-i, --watch-poll-interval=1000: Interval (in ms) between two checks for file changes
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## Examples

#### Launch with port-forwarding
```
amp debug --port-forward
```

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_AUTO_BUILD` (same as `--auto-build`)
* `AMP_AUTO_CREATE_CONFIG` (same as `--auto-create-config`)
* `AMP_AUTO_DEPLOY` (same as `--auto-deploy`)
* `AMP_AUTO_SYNC` (same as `--auto-sync`)
* `AMP_BUILD_CONCURRENCY` (same as `--build-concurrency`)
* `AMP_CACHE_ARTIFACTS` (same as `--cache-artifacts`)
* `AMP_CACHE_FILE` (same as `--cache-file`)
* `AMP_CLEANUP` (same as `--cleanup`)
* `AMP_CONFIG` (same as `--config`)
* `AMP_DEFAULT_REPO` (same as `--default-repo`)
* `AMP_DETECT_MINIKUBE` (same as `--detect-minikube`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_FORCE` (same as `--force`)
* `AMP_HYDRATION_DIR` (same as `--hydration-dir`)
* `AMP_INSECURE_REGISTRY` (same as `--insecure-registry`)
* `AMP_ITERATIVE_STATUS_CHECK` (same as `--iterative-status-check`)
* `AMP_KUBE_CONTEXT` (same as `--kube-context`)
* `AMP_KUBECONFIG` (same as `--kubeconfig`)
* `AMP_LABEL` (same as `--label`)
* `AMP_MODULE` (same as `--module`)
* `AMP_MUTE_LOGS` (same as `--mute-logs`)
* `AMP_NAMESPACE` (same as `--namespace`)
* `AMP_NO_PRUNE` (same as `--no-prune`)
* `AMP_NO_PRUNE_CHILDREN` (same as `--no-prune-children`)
* `AMP_PLATFORM` (same as `--platform`)
* `AMP_PORT_FORWARD` (same as `--port-forward`)
* `AMP_PROFILE` (same as `--profile`)
* `AMP_PROFILE_AUTO_ACTIVATION` (same as `--profile-auto-activation`)
* `AMP_PROPAGATE_PROFILES` (same as `--propagate-profiles`)
* `AMP_PROTOCOLS` (same as `--protocols`)
* `AMP_REMOTE_CACHE_DIR` (same as `--remote-cache-dir`)
* `AMP_RESOURCE_SELECTOR_RULES_FILE` (same as
  `--resource-selector-rules-file`)
* `AMP_RPC_HTTP_PORT` (same as `--rpc-http-port`)
* `AMP_RPC_PORT` (same as `--rpc-port`)
* `AMP_SKIP_TESTS` (same as `--skip-tests`)
* `AMP_STATUS_CHECK` (same as `--status-check`)
* `AMP_SYNC_REMOTE_CACHE` (same as `--sync-remote-cache`)
* `AMP_TAG` (same as `--tag`)
* `AMP_TAIL` (same as `--tail`)
* `AMP_TOOT` (same as `--toot`)
* `AMP_TRIGGER` (same as `--trigger`)
* `AMP_WAIT_FOR_CONNECTION` (same as `--wait-for-connection`)
* `AMP_WAIT_FOR_DELETIONS` (same as `--wait-for-deletions`)
* `AMP_WAIT_FOR_DELETIONS_DELAY` (same as `--wait-for-deletions-delay`)
* `AMP_WAIT_FOR_DELETIONS_MAX` (same as `--wait-for-deletions-max`)
* `AMP_WATCH_IMAGE` (same as `--watch-image`)
* `AMP_WATCH_POLL_INTERVAL` (same as `--watch-poll-interval`)