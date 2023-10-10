+++
title = "入门指南"
description = "我们的入门指南将引导您完成一系列安装、初始化、实验和开始使用 Amphitheatre 的步骤"
weight = 0
sort_by = "weight"

[extra.sidebar]
linkable = true
+++


## 1. 安装 API 服务器

一旦正确设置了 **Helm**，请按如下方式添加存储库并安装图表，使用发布名称 `amp`：

```sh
helm repo add amphitheatre https://charts.amphitheatre.app
helm install amp amphitheatre/amphitheatre --create-namespace --namespace amp-system
```

有关更高级的安装配置，请参阅 [安装 API 服务器](@/installation/api-server.zh.md)。

## 2. 配置集群

安装 Amphitheatre 后，您需要初始化一些配置，其中一个更重要的配置是凭据的配置。创建一个 `Secret`，用于 Docker Registry 的推送凭据，您计划使用 [`Builder`](@/concepts/builders.zh.md) 发布 OCI 镜像。您的配置应该如下所示：

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

将该配置应用到集群

```bash
kubectl apply -n amp-system -f amp-credentials.yaml
```

有关更高级的配置，请参阅 [配置 Amphitheatre](@/installation/configuration.zh.md).

## 3. 安装 CLI

Amphitheatre CLI 可以通过 Linux、macOS 和 Windows 上的各种包管理器进行安装。预编译的二进制文件可从 [**GitHub 发布页面**](https://github.com/amphitheatre-app/cli/releases) 获取。只需下载适当的二进制文件并将其添加到您的 `PATH`。

有关详细的安装说明，请按照您的操作系统的指南进行操作。在终端中复制并粘贴以下命令中的一个：

```
# 对于 x86_64 (amd64) 的 Linux
curl -L https://github.com/amphitheatre-app/cli/releases/download/latest/amp-linux-amd64.tar.gz && \
tar -xzf amp-linux-amd64.tar.gz && \
sudo install amp /usr/local/bin/
```

```
# 对于 aarch64 (arm64) 的 macOS
curl -L https://github.com/amphitheatre-app/cli/releases/download/latest/amp-darwin-arm64.tar.gz && \
tar -xzf amp-darwin-arm64.tar.gz && \
sudo install amp /usr/local/bin/
```

```
docker run ghcr.io/amphitheatre-app/amp:latest amp <command>
```

安装 Amphitheatre CLI 后，使用以下命令验证是否正确安装：

```
amp version
```

有关更高级的安装配置，请参阅 [安装 CLI](@/installation/cli.zh.md)。

## 4. 快速入门

使用我们的快速入门，其中包含代码示例，旨在让您快速开始使用 Amphitheatre。所有模板都预配置为使用 Amphitheatre 并准备好进行编码：

{{ grid(columns=3)}}
{{ column() }}{{ card(title="Golang", text="学习如何部署 Go 应用程序。", url="/zh/examples/golang/") }}{{ end() }}
{{ column() }}{{ card(title="Python", text="学习如何部署 Python 应用程序。", url="/zh/examples/python/") }}{{ end() }}
{{ column() }}{{ card(title="Java", text="学习如何部署 Java 应用程序。", url="/zh/examples/java/") }}{{ end() }}
{{ column() }}{{ card(title="NodeJs", text="学习如何部署 NodeJs 应用程序。", url="/zh/examples/nodejs") }}{{ end() }}
{{ column() }}{{ card(title="Rust", text="学习如何部署 Rust 应用程序。", url="/zh/examples/rust") }}{{ end() }}
{{ column() }}{{ card(title="PHP", text="学习如何部署 PHP 应用程序。", url="/zh/examples/php") }}{{ end() }}
{{ end() }}

#### 告诉我们您的看法！

我们不断努力改进我们的快速入门示例，并重视您的反馈意见。如果您在按照这些步骤操作时遇到任何问题，请随时在 Amphitheatre Discord 社区中与我们联系。
