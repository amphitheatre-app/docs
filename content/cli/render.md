+++
title = "amp render"
weight = 1
+++

Perform all image builds, and output rendered Kubernetes manifests


## Usage
```
amp render [options]
```

## Options

```
    --assume-yes=false: If true, amp will skip yes/no confirmation from the user and default to yes
-a, --build-artifacts=: File containing build result from a previous 'amp build --file-output'
    --cache-artifacts=true: Set to false to disable default caching of artifacts
-d, --default-repo='': Default repository value (overrides global config)
    --digest-source='remote': Set to 'remote' to skip builds and resolve the digest of images by tag from the remote registry. Set to 'local' to build images locally and use digests from built images. Set to 'tag' to use tags directly from the build. Set to 'none' to use tags directly from the Kubernetes manifests.
-f, --filename='.amp.toml': Path or URL to the Amphitheatre config file
    --hydration-dir='.kpt-pipeline': The directory to where the (kpt) hydration takes place. Default to a hidden directory .kpt-pipeline.
-i, --images=: A list of pre-built images to deploy, either tagged images or NAME=TAG pairs
-l, --label=[]: Add custom labels to deployed objects. Set multiple times for multiple labels
    --loud=false: Show the build logs and output
-m, --module=[]: Filter Amphitheatre configs to only the provided named modules
-n, --namespace='': Run deployments in the specified namespace
    --offline=false: Do not connect to Kubernetes API server for manifest creation and validation. This is helpful when no Kubernetes cluster is available (e.g. GitOps model). No metadata.namespace attribute is injected in this case - the manifest content does not get changed.
-o, --output='': File to write rendered manifests to
-p, --profile=[]: Activate profiles by name (prefixed with `-` to disable a profile)
    --profile-auto-activation=true: Set to false to disable profile auto activation
    --propagate-profiles=true: Setting '--propagate-profiles=false' disables propagating profiles set by the '--profile' flag across config dependencies. This mean that only profiles defined directly in the target '.amp.toml' file are activated.
    --remote-cache-dir='': Specify the location of the git repositories cache (default $HOME/.amp/repos)
    --resource-selector-rules-file='': Path to JSON file specifying the deny list of yaml objects for amp to NOT transform with 'image' and 'label' field replacements.  NOTE: this list is additive to amp's default denylist and denylist has priority over allowlist
    --sync-remote-cache='always': Controls how Amphitheatre manages the remote config cache (see `remote-cache-dir`). One of `always` (default), `missing`, or `never`. `always` syncs remote repositories to latest on access. `missing` only clones remote repositories if they do not exist locally. `never` means the user takes responsibility for updating remote repositories.
    --wait-for-connection=false: Blocks ending execution of amp until the /v2/events gRPC/HTTP endpoint is hit
```

> Use "amp options" for a list of global command-line options (applies to all commands).


## Examples
  
#### Hydrate Kubernetes manifests without building the images, using digest resolved from tag in remote registry 
```
amp render --digest-source=remote
```

## Environment vars

* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_BUILD_ARTIFACTS` (same as `--build-artifacts`)
* `AMP_CACHE_ARTIFACTS` (same as `--cache-artifacts`)
* `AMP_DEFAULT_REPO` (same as `--default-repo`)
* `AMP_DIGEST_SOURCE` (same as `--digest-source`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_HYDRATION_DIR` (same as `--hydration-dir`)
* `AMP_IMAGES` (same as `--images`)
* `AMP_LABEL` (same as `--label`)
* `AMP_LOUD` (same as `--loud`)
* `AMP_MODULE` (same as `--module`)
* `AMP_NAMESPACE` (same as `--namespace`)
* `AMP_OFFLINE` (same as `--offline`)
* `AMP_OUTPUT` (same as `--output`)
* `AMP_PROFILE` (same as `--profile`)
* `AMP_PROFILE_AUTO_ACTIVATION` (same as `--profile-auto-activation`)
* `AMP_PROPAGATE_PROFILES` (same as `--propagate-profiles`)
* `AMP_REMOTE_CACHE_DIR` (same as `--remote-cache-dir`)
* `AMP_RESOURCE_SELECTOR_RULES_FILE` (same as
  `--resource-selector-rules-file`)
* `AMP_SYNC_REMOTE_CACHE` (same as `--sync-remote-cache`)
* `AMP_WAIT_FOR_CONNECTION` (same as `--wait-for-connection`)