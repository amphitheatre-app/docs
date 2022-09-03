+++
title = "amp init"
weight = 1
+++

Generate configuration for deploying an application


## Usage
```
amp init [options]
```

## Options
```
    --analyze=false: Print all discoverable Dockerfiles and images in JSON format to stdout
-a, --artifact=[]: '='-delimited Dockerfile/image pair, or JSON string, to generate build artifact
(example: --artifact='{"builder":"Docker","payload":{"path":"/web/Dockerfile.web"},"image":"gcr.io/web-project/image"}')
    --assume-yes=false: If true, amp will skip yes/no confirmation from the user and default to yes
    --compose-file='': Initialize from a docker-compose file
    --default-kustomization='': Default Kustomization overlay path (others will be added as profiles)
-f, --filename='.amp.toml': Path or URL to the Amphitheatre config file
    --force=false: Force the generation of the Amphitheatre config
    --generate-manifests=false: Allows amp to try and generate basic kubernetes resources to get your project started
-k, --kubernetes-manifest=[]: A path or a glob pattern to kubernetes manifests (can be non-existent) to be added to the kubectl deployer (overrides detection of kubernetes manifests). Repeat the flag for multiple entries. E.g.: amp init -k pod.yaml -k k8s/*.yml
-m, --module=[]: Filter Amphitheatre configs to only the provided named modules
    --remote-cache-dir='': Specify the location of the git repositories cache (default $HOME/.amp/repos)
    --skip-build=false: Skip generating build artifacts in Amphitheatre config
    --sync-remote-cache='always': Controls how Amphitheatre manages the remote config cache (see `remote-cache-dir`). One of `always` (default), `missing`, or `never`. `always` syncs remote repositories to latest on access. `missing` only clones remote repositories if they do not exist locally. `never` means the user takes responsibility for updating remote repositories.
```

> Use "amp options" for a list of global command-line options (applies to all commands).

## Environment vars

* `AMP_ANALYZE` (same as `--analyze`)
* `AMP_ARTIFACT` (same as `--artifact`)
* `AMP_ASSUME_YES` (same as `--assume-yes`)
* `AMP_COMPOSE_FILE` (same as `--compose-file`)
* `AMP_DEFAULT_KUSTOMIZATION` (same as `--default-kustomization`)
* `AMP_FILENAME` (same as `--filename`)
* `AMP_FORCE` (same as `--force`)
* `AMP_GENERATE_MANIFESTS` (same as `--generate-manifests`)
* `AMP_KUBERNETES_MANIFEST` (same as `--kubernetes-manifest`)
* `AMP_MODULE` (same as `--module`)
* `AMP_REMOTE_CACHE_DIR` (same as `--remote-cache-dir`)
* `AMP_SKIP_BUILD` (same as `--skip-build`)
* `AMP_SYNC_REMOTE_CACHE` (same as `--sync-remote-cache`)