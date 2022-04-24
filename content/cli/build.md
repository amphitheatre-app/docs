+++
title = "amp build"
weight = 1
+++

Build the artifacts

## Usage
```
amp build [options]
```

## Options

```
    --assume-yes=false: If true, amp will skip yes/no confirmation from the user and default to yes
    --build-concurrency=-1: Number of concurrently running builds. Set to 0 to run all builds in parallel. Doesn't violate build order among dependencies.
-b, --build-image=[]: Only build artifacts with image names that contain the given substring. Default is to build sources for all artifacts
    --cache-artifacts=true: Set to false to disable default caching of artifacts
    --cache-file='': Specify the location of the cache file (default $HOME/.amp/cache)
-c, --config='': File for global configurations (defaults to $HOME/.amp/config)
-d, --default-repo='': Default repository value (overrides global config)
    --detect-minikube=true: Use heuristics to detect a minikube cluster
    --dry-run=false: Don't build images, just compute the tag for each artifact.
    --file-output='': Filename to write build images to
-f, --filename='.amp.yaml': Path or URL to the Amphitheatre config file
    --insecure-registry=[]: Target registries for built images which are not secure
    --kube-context='': Deploy to this Kubernetes context
    --kubeconfig='': Path to the kubeconfig file to use for CLI requests.
-m, --module=[]: Filter Amphitheatre configs to only the provided named modules
    --mute-logs=[]: mute logs for specified stages in pipeline (build, deploy, status-check, none, all)
-n, --namespace='': Run deployments in the specified namespace
-o, --output={{json .}}: Used in conjunction with --quiet flag. Format output with go-template. For full struct documentation, see https://godoc.org/github.com/GoogleContainerTools/amp/cmd/amp/app/flags#BuildOutput
    --platform=[]: The platform to target for the build artifacts
-p, --profile=[]: Activate profiles by name (prefixed with `-` to disable a profile)
    --profile-auto-activation=true: Set to false to disable profile auto activation
    --propagate-profiles=true: Setting '--propagate-profiles=false' disables propagating profiles set by the '--profile' flag across config dependencies. This mean that only profiles defined directly in the target '.amp.yaml' file are activated.
    --push=: Push the built images to the specified image repository.
-q, --quiet=false: Suppress the build output and print image built on success. See --output to format output.
    --remote-cache-dir='': Specify the location of the git repositories cache (default $HOME/.amp/repos)
    --rpc-http-port=: tcp port to expose the Amphitheatre API over HTTP REST
    --rpc-port=: tcp port to expose the Amphitheatre API over gRPC
    --skip-tests=false: Whether to skip the tests after building
    --sync-remote-cache='always': Controls how Amphitheatre manages the remote config cache (see `remote-cache-dir`). One of `always` (default), `missing`, or `never`. `always` syncs remote repositories to latest on access. `missing` only clones remote repositories if they do not exist locally. `never` means the user takes responsibility for updating remote repositories.
-t, --tag='': The optional custom tag to use for images which overrides the current Tagger configuration
    --toot=false: Emit a terminal beep after the deploy is complete
    --wait-for-connection=false: Blocks ending execution of amp until the /v2/events gRPC/HTTP endpoint is hit
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## Examples

#### Build all the artifacts
```
amp build
```
#### Build artifacts with a profile activated
```
amp build -p <profile>
```
#### Build artifacts whose image name contains <db>
```
amp build -b <db>
```
#### Quietly build artifacts and output the image names as json
```
amp build -q > build_result.json
```
#### Build the artifacts and then deploy them
```
amp build -q | amp deploy --build-artifacts -
```
#### Print the final image names
```
amp build -q --dry-run
```

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_BUILD_CONCURRENCY` (same as `--build-concurrency`)
* `AMP_BUILD_IMAGE` (same as `--build-image`)
* `AMP_CACHE_ARTIFACTS` (same as `--cache-artifacts`)
* `AMP_CACHE_FILE` (same as `--cache-file`)
* `AMP_CONFIG` (same as `--config`)
* `AMP_DEFAULT_REPO` (same as `--default-repo`)
* `AMP_DETECT_MINIKUBE` (same as `--detect-minikube`)
* `AMP_DRY_RUN` (same as `--dry-run`)
* `AMP_FILE_OUTPUT` (same as `--file-output`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_INSECURE_REGISTRY` (same as `--insecure-registry`)
* `AMP_KUBE_CONTEXT` (same as `--kube-context`)
* `AMP_KUBECONFIG` (same as `--kubeconfig`)
* `AMP_MODULE` (same as `--module`)
* `AMP_MUTE_LOGS` (same as `--mute-logs`)
* `AMP_NAMESPACE` (same as `--namespace`)
* `AMP_OUTPUT` (same as `--output`)
* `AMP_PLATFORM` (same as `--platform`)
* `AMP_PROFILE` (same as `--profile`)
* `AMP_PROFILE_AUTO_ACTIVATION` (same as `--profile-auto-activation`)
* `AMP_PROPAGATE_PROFILES` (same as `--propagate-profiles`)
* `AMP_PUSH` (same as `--push`)
* `AMP_QUIET` (same as `--quiet`)
* `AMP_REMOTE_CACHE_DIR` (same as `--remote-cache-dir`)
* `AMP_RPC_HTTP_PORT` (same as `--rpc-http-port`)
* `AMP_RPC_PORT` (same as `--rpc-port`)
* `AMP_SKIP_TESTS` (same as `--skip-tests`)
* `AMP_SYNC_REMOTE_CACHE` (same as `--sync-remote-cache`)
* `AMP_TAG` (same as `--tag`)
* `AMP_TOOT` (same as `--toot`)
* `AMP_WAIT_FOR_CONNECTION` (same as `--wait-for-connection`)