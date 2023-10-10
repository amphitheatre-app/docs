+++
title = "安装命令行工具"
description = "安装 Amphitheatre 命令行工具。"
weight = 3

[extra]
toc = false
+++

Amphitheatre CLI 可以使用 Linux、macOS 和 Windows 上的各种包管理器进行安装。
预编译的二进制文件可从 [**GitHub 发布页面**](https://github.com/amphitheatre-app/cli/releases) 获取，
只需下载适当的二进制文件并将其添加到您的 `PATH`.

有关详细的安装说明，请按照适用于您操作系统的指南进行操作。

{{ tabs(id="os", items=["Linux", "macOS", "Windows", "Docker"])}}

{{ pane(tab="os", index=1) }}

在终端中复制粘贴以下命令之一：

```
# For Linux on x86_64 (amd64)
curl -L https://github.com/amphitheatre-app/cli/releases/download/latest/amp-linux-amd64.tar.gz && \
tar -xzf amp-linux-amd64.tar.gz && \
sudo install amp /usr/local/bin/
```

```
# For Linux on aarch64 (arm64)
curl -L https://github.com/amphitheatre-app/cli/releases/download/latest/amp-linux-arm64.tar.gz && \
tar -xzf amp-linux-arm64.tar.gz && \
sudo install amp /usr/local/bin/
```

{{ end() }}

{{ pane(tab="os", index=2) }}

在终端中复制粘贴以下命令之一：

```
# For macOS on x86_64 (amd64)
curl -L https://github.com/amphitheatre-app/cli/releases/download/latest/amp-darwin-amd64.tar.gz && \
tar -xzf amp-darwin-amd64.tar.gz && \
sudo install amp /usr/local/bin/
```

```
# For macOS on aarch64 (arm64)
curl -L https://github.com/amphitheatre-app/cli/releases/download/latest/amp-darwin-arm64.tar.gz && \
tar -xzf amp-darwin-arm64.tar.gz && \
sudo install amp /usr/local/bin/
```

Amphitheatre 命令行工具也会定期更新到一些中央包管理器中：

### Homebrew

```
brew install amp
```

### MacPorts

```
sudo port install amp
```

{{ end() }}

{{ pane(tab="os", index=3) }}

1. 首先，访问以下链接以下载文件：

    [https://github.com/amphitheatre-app/cli/releases/download/latest/amp-windows-amd64.zip](https://github.com/amphitheatre-app/cli/releases/download/latest/amp-windows-amd64.zip)

    单击链接，浏览器将开始下载压缩文件（`amp-windows-amd64.zip`）。

2. 一旦下载完成，您可以将该文件解压缩到您喜欢的目录中。您可以使用操作系统内置的解压工具或第三方压缩工具（如 7-Zip 或 WinRAR ）来完成此操作。

3. 解压缩后，您将获得一个名为 `amp.exe` 的可执行文件。

最后，将 `amp.exe` 文件添加到您的 `PATH` 环境变量中，这样您就可以在任何位置方便地运行 `amp` 命令了。

### Scoop

Amphitheatre 命令行工具可以使用 `Scoop` 包管理器从 `extras` 存储桶安装。此包并非 Amphitheatre 团队维护。

```
scoop bucket add extras
scoop install amp
```

### Chocolatey

Amphitheatre 命令行工具可以使用 `Chocolatey` 包管理器安装。此包并非 Amphitheatre 团队维护。

> **注意**\
Chocolatey 的安装机制会干扰 `Ctrl+C` 处理并阻止 `amp` 清理部署。这无法通过 Amphitheatre 命令行工具修复。有关此缺陷的更多信息，请参阅 `chocolatey/shimgen#32`。

```
choco install -y amp
```

{{ end() }}

{{ pane(tab="os", index=4) }}

对于最新的稳定版本，您可以使用以下命令：

```
docker run ghcr.io/amphitheatre-app/amp:latest amp <command>
```

{{ end() }}

## 验证

安装 Amphitheatre 命令行工具后，请使用以下命令验证是否正确安装：

```
amp version
```

## 故障排除

如果在安装 Amphitheatre 命令行工具时遇到任何问题，请查看故障排除 FAQ 中的错误消息。
