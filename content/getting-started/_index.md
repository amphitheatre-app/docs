+++
title = "Getting Started"
description = "Our getting started guide will walk you through a series of steps to install, initialize, experiment with, and start using Amphitheatre."
weight = 0
sort_by = "weight"

[extra.sidebar]
linkable = true
+++

## 1. Install API Server

Once **Helm** has been set up correctly, add the repo and install the chart with
the release name `amp` as follows:

```sh
helm repo add amphitheatre https://charts.amphitheatre.app
helm install amp amphitheatre/amphitheatre --create-namespace --namespace amp-system
```

For more advanced installation configurations see [Installing API
Server](@/installation/api-server.md).

## 2. Configure the cluster

After installing Amphitheatre, you need to initialize some configurations, one
of the more important ones being the configuration of credentials.

Create a `Secret` with push credentials for the Docker Registry that you plan on
publishing OCI images to with [`Builder`](@/concepts/builders.md). Your
configuration create should look something like this:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: amp-credentials
  namespace: amp-system
stringData:
  credentials: |
    [[registries]]
    name = "Docker Hub"
    default = true
    server = "https://index.docker.io/v1/"
    username = "<username>"
    password = "<password>"
    token = "<token>"

    [[repositories]]
    name = "GitHub"
    driver = "github"
    server = "https://github.com"
    username = "<username>"
    password = "<password>"
    token = "<token>"
```

Apply that configuration to the cluster

```bash
kubectl apply -n amp-system -f amp-credentials.yaml
```

For more advanced configurations see [Configure the
Amphitheatre](@/installation/configuration.md).

## 3. Install CLI

Amphitheatre CLI can be installed by using various package managers on Linux,
macOS and Windows. The precompiled binaries are available from the [**GitHub
releases page**](https://github.com/amphitheatre-app/cli/releases). Simply
download the appropriate binary and add it to your `PATH`.

For detailed install instructions, follow the guide for your operating system.
Copy and paste one of the following commands in your terminal:

```
# For Linux on x86_64 (amd64)
curl -L https://github.com/amphitheatre-app/cli/releases/download/latest/amp-linux-amd64.tar.gz && \
tar -xzf amp-linux-amd64.tar.gz && \
sudo install amp /usr/local/bin/
```

```
# For macOS on aarch64 (arm64)
curl -L https://github.com/amphitheatre-app/cli/releases/download/latest/amp-darwin-arm64.tar.gz && \
tar -xzf amp-darwin-arm64.tar.gz && \
sudo install amp /usr/local/bin/
```

```
docker run ghcr.io/amphitheatre-app/amp:latest <command>
```

After you install Amphitheatre CLI, verify that you installed it correctly with:

```
amp version
```

For more advanced installation configurations see [Installing CLI](@/installation/cli.md).

## 4. Quick start

Hit the ground running with our quick starts, complete with code samples aimed
to get you started quickly with Amphitheatre. All templates are pre-configured
to use Amphitheatre and ready-to-code:

{{ grid(columns=3)}}
{{ column() }}{{ card(title="Golang", text="Learn how to deploy a Go application.", url="/examples/golang/") }}{{ end() }}
{{ column() }}{{ card(title="Python", text="Learn how to deploy a Python application.", url="/examples/python/") }}{{ end() }}
{{ column() }}{{ card(title="Java", text="Learn how to deploy a Java application.", url="/examples/java/") }}{{ end() }}
{{ column() }}{{ card(title="NodeJs", text="Learn how to deploy a NodeJs application.", url="/examples/nodejs") }}{{ end() }}
{{ column() }}{{ card(title="Rust", text="Learn how to deploy a Rust application.", url="/examples/rust") }}{{ end() }}
{{ column() }}{{ card(title="PHP", text="Learn how to deploy a PHP application.", url="/examples/php") }}{{ end() }}
{{ end() }}

#### Tell us what you think!

We’re continuously working to improve our Quickstart examples and value your
feedback. If you run into any issues while following these steps, please reach
out to us in the Amphitheatre Discord community.
