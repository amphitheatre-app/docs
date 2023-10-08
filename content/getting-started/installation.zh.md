+++
title = "安装 Amphitheatre"
description = "Amphitheatre 快速安装，一键完成"
weight = 1

[extra.sidebar]
linkable = true
title = "快速安装 Amphitheatre"
+++

本页面描述了如何快速安装 Amphitheatre，仅用于后续的快速入门相关任务。有关更多信息，请参阅[安装指南](@/installation/_index.zh.md)文档。

以下是您将要执行的安装工作流程：

## 1. 安装 API 服务器

一旦正确设置了 Helm，可以按照以下方式添加 repo：

```
helm repo add amphitheatre https://charts.amphitheatre.app
```

使用名称 amp 安装图表：

```
helm install amp amphitheatre/amphitheatre --create-namespace --namespace amp-system
```

Metrics Server是 Kubernetes 中用于监视容器资源使用情况的关键组件，并作为 Amphitheatre 项目中“统计”功能的前提依赖项。在 Helm Chart 中，`metrics-server.enabled` 参数默认为 `true`，确保在 Amphitheatre 安装期间自动安装 Metrics Server。

但是，如果您的集群已经运行全局 Metrics Server，并且希望避免重复安装，可以在安装 Amphitheatre 时通过设置`--set metrics-server.enabled=false` 参数来禁用 Metrics Server 的安装。

有关更高级的安装配置，请参阅[安装API服务器](@/installation/api-server.md)。

## 2. 安装桌面应用程序

安装方式取决于您的操作系统，请选择下面特定操作系统的说明：

{{ grid(columns=3) }}
{{ column() }}{{ card(title="macOS 上的 Amphitheatre 桌面版", url="https://amphitheatre.app/") }}{{ end() }}
{{ column() }}{{ card(title="Windows 上的 Amphitheatre 桌面版", url="https://amphitheatre.app/") }}{{ end() }}
{{ column() }}{{ card(title="Ubuntu 上的 Amphitheatre 桌面版", url="https://repo.amphitheatre.app/desktop/releases/latest/amphitheatre-desktop.deb") }}{{ end() }}
{{ end() }}

## 3. 安装命令行工具（CLI）

命令行工具（CLI）是**可选的**，可以根据您的需求安装或不安装。有关安装说明，请按照您的操作系统的指南进行操作：

{{ tabs(id="os", items=["macOS", "Windows", "Linux", "Docker"])}}

{{ pane(tab="os", index=1) }}

只需下载适用于您操作系统的二进制文件并将其添加到 `PATH` 中。或者，将以下命令之一复制+粘贴到终端中：

```
# 对于 macOS 上的 x86_64（amd64）
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-amd64 && \
sudo install amp /usr/local/bin/
```

```
# 对于 macOS 上的 ARMv8（arm64）
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-arm64 && \
sudo install amp /usr/local/bin/
```

Amphitheatre CLI 还可以通过 Homebrew 软件包管理器保持更新：

```
brew install amp
```

{{ end() }}

{{ pane(tab="os", index=2) }}

最新的稳定版本二进制文件可以在这里找到：

[https://repo.amphitheatre.app/cli/releases/latest/amp-windows-amd64.exe](https://repo.amphitheatre.app/cli/releases/latest/amp-windows-amd64.exe)

只需下载并将其放置在 `PATH` 中，命名为 `amp.exe`。

Amphitheatre CLI 可以使用 Scoop 或 Chocolatey 软件包管理器进行安装：

```
# 对于 scoop
scoop bucket add extras
scoop install amp

# 对于 Chocolatey
choco install -y amp
```

{{ end() }}

{{ pane(tab="os", index=3) }}

只需下载适用于您操作系统的二进制文件并将其添加到 `PATH` 中。或者，将以下命令之一复制+粘贴到终端中：

```
# 对于 Linux x86_64（amd64）
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-linux-amd64 && \
sudo install amp /usr/local/bin/
```

```
# 对于 Linux ARMv8（arm64）
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-linux-arm64 && \
sudo install amp /usr/local/bin/
```

{{ end() }}

{{ pane(tab="os", index=4) }}

要使用最新的稳定版本，请执行：

```
docker run amphitheatre/amp:latest amp <command>
```

{{ end() }}

## 验证

安装 Amphitheatre CLI 后，请使用以下命令验证是否已正确安装：

```
amp version
```
