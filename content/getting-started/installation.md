+++
title = "Installing Amphitheatre"
description = "Quick installation of Amphitheatre just in a single page"
weight = 1

[extra.sidebar]
linkable = true
title = "Installing Amphitheatre quickly"
+++

This page describes how to quickly install Amphitheatre, which is only used for
subsequent quick start related tasks. For more information please see the
[installation guide](@/installation/_index.md) documentation.

This is the installation workflow you will follow:

## 1. Install API Server

Once Helm has been set up correctly, add the repo as follows:

```
  helm repo add amphitheatre https://charts.amphitheatre.app
```

To install the chart with the release name amp:

```
helm install amp amphitheatre/amphitheatre --create-namespace --namespace amp-system
```

Metrics Server is a crucial component in Kubernetes for monitoring container
resource usage and serves as a prerequisite dependency for the Stats feature in
the Amphitheatre project. In the Helm Chart, the `metrics-server.enabled`
parameter defaults to true, ensuring Metrics Server is automatically installed
during the Amphitheatre installation.

However, if your cluster already has a global Metrics Server running and you
wish to avoid redundant installation, you can disable the installation of
Metrics Server by setting the `--set metrics-server.enabled=false` parameter
when installing Amphitheatre.

For more advanced installation configurations see [Installing API Server](@/installation/api-server.md).

## 2. Install Desktop Application

Installation varies depending on your operating system, please select the
specific operating system instructions below:

{{ grid(columns=3) }}
{{ column() }}{{ card(title="Amphitheatre Desktop for macOS", url="https://amphitheatre.app/") }}{{ end() }}
{{ column() }}{{ card(title="Amphitheatre Desktop for Windows", url="https://amphitheatre.app/") }}{{ end() }}
{{ column() }}{{ card(title="Amphitheatre Desktop for Ubuntu", url="https://repo.amphitheatre.app/desktop/releases/latest/amphitheatre-desktop.deb") }}{{ end() }}
{{ end() }}

## 3. Install CLI

The CLI is **optional** and can be installed or not depending on your own needs. For
install instructions, follow the guide for your operating system:

{{ tabs(id="os", items=["macOS", "Windows", "Linux", "Docker"])}}

{{ pane(tab="os", index=1) }}

Simply download the appropriate binary and add it to your PATH. Or, copy+paste
one of the following commands in your terminal:

```
# For macOS on x86_64 (amd64)
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-amd64 && \
sudo install amp /usr/local/bin/
```

```
# For macOS on ARMv8 (arm64)
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-arm64 && \
sudo install amp /usr/local/bin/
```

Amphitheatre CLI is also kept up to date on Homebrew package manager:

```
brew install amp
```

{{ end() }}

{{ pane(tab="os", index=2) }}

The latest stable release binary can be found here:

[https://repo.amphitheatre.app/cli/releases/latest/amp-windows-amd64.exe](https://repo.amphitheatre.app/cli/releases/latest/amp-windows-amd64.exe)

Simply download it and place it in your `PATH` as `amp.exe`.

Amphitheatre CLI can be installed using the Scoop or Chocolatey package manager:

```
# for scoop
scoop bucket add extras
scoop install amp

# for Chocolatey
choco install -y amp
```

{{ end() }}

{{ pane(tab="os", index=3) }}

Simply download the appropriate binary and add it to your PATH. Or, copy+paste
one of the following commands in your terminal:

```
# For Linux x86_64 (amd64)
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-linux-amd64 && \
sudo install amp /usr/local/bin/
```

```
# For Linux ARMv8 (arm64)
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-linux-arm64 && \
sudo install amp /usr/local/bin/
```

{{ end() }}

{{ pane(tab="os", index=4) }}

For the latest stable release, you can use:

```
docker run amphitheatre/amp:latest amp <command>
```

{{ end() }}

#### Verify

After you install Amphitheatre CLI, verify that you installed it correctly with:

```
amp version
```
