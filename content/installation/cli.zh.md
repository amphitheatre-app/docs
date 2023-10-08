+++
title = "安装命令行工具"
description = "安装 Amphitheatre 命令行工具。"
weight = 3

[extra]
toc = false
+++

有关详细的安装说明，请按照适用于您操作系统的指南进行操作。

{{ tabs(id="os", items=["Linux", "macOS", "Windows", "Docker"])}}

{{ pane(tab="os", index=1) }}

最新的稳定版本二进制文件可以在以下位置找到：

- Linux x86_64 (amd64): [https://repo.amphitheatre.app/cli/releases/latest/amp-linux-amd64](https://repo.amphitheatre.app/cli/releases/latest/amp-linux-amd64)
- Linux ARMv8 (arm64): [https://repo.amphitheatre.app/cli/releases/latest/amp-linux-arm64](https://repo.amphitheatre.app/cli/releases/latest/amp-linux-arm64)

只需下载适当的二进制文件并将其添加到您的 `PATH` 中。或者，在终端中复制粘贴以下命令之一：

```
# 对于 Linux x86_64 (amd64)
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-linux-amd64 && \
sudo install amp /usr/local/bin/
```

```
# 对于 Linux ARMv8 (arm64)
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-linux-arm64 && \
sudo install amp /usr/local/bin/
```

我们还发布了最新提交构建的 `bleeding edge` 版本：

- Linux x86_64 (amd64): [https://repo.amphitheatre.app/cli/builds/latest/amp-linux-amd64](https://repo.amphitheatre.app/cli/builds/latest/amp-linux-amd64)
- Linux ARMv8 (arm64): [https://repo.amphitheatre.app/cli/builds/latest/amp-linux-arm64](https://repo.amphitheatre.app/cli/builds/latest/amp-linux-arm64)

```
# 对于 Linux x86_64 (amd64)
curl -Lo amp https://repo.amphitheatre.app/cli/builds/latest/amp-linux-amd64 && \
sudo install amp /usr/local/bin/
```

```
# 对于 Linux ARMv8 (arm64)
curl -Lo amp https://repo.amphitheatre.app/cli/builds/latest/amp-linux-arm64 && \
sudo install amp /usr/local/bin/
```

{{ end() }}

{{ pane(tab="os", index=2) }}

最新的稳定版本二进制文件可以在以下位置找到：

- Darwin x86_64 (amd64): [https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-amd64](https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-amd64)
- Darwin ARMv8 (arm64): [https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-arm64](https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-arm64)

只需下载适当的二进制文件并将其添加到您的 `PATH` 中。或者，在终端中复制粘贴以下命令之一：

```
# 对于 macOS x86_64 (amd64)
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-amd64 && \
sudo install amp /usr/local/bin/
```

```
# 对于 macOS ARMv8 (arm64)
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-arm64 && \
sudo install amp /usr/local/bin/
```

我们还发布了最新提交构建的 `bleeding edge` 版本：

- Darwin x86_64 (amd64): [https://repo.amphitheatre.app/cli/builds/latest/amp-darwin-amd64](https://repo.amphitheatre.app/cli/builds/latest/amp-darwin-amd64)
- Darwin ARMv8 (arm64): [https://repo.amphitheatre.app/cli/builds/latest/amp-darwin-arm64](https://repo.amphitheatre.app/cli/builds/latest/amp-darwin-arm64)

```
# 对于 macOS x86_64 (amd64)
curl -Lo amp https://repo.amphitheatre.app/cli/builds/latest/amp-darwin-amd64 && \
sudo install amp /usr/local/bin/
```

```
# 对于 macOS ARMv8 (arm64)
curl -Lo amp https://repo.amphitheatre.app/cli/builds/latest/amp-darwin-arm64 && \
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

最新的稳定版本二进制文件可以在以下位置找到：

[https://repo.amphitheatre.app/cli/releases/latest/amp-windows-amd64.exe](https://repo.amphitheatre.app/cli/releases/latest/amp-windows-amd64.exe)

只需下载它并将其放在您的 `PATH` 中，命名为 `amp.exe`。

我们还发布了 **bleeding edge** 版本，它是从最新提交构建的：

[https://repo.amphitheatre.app/cli/builds/latest/amp-windows-amd64.exe](https://repo.amphitheatre.app/cli/builds/latest/amp-windows-amd64.exe)

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
docker run amphitheatre/amp:latest amp <command>
```

### Bleeding edge 二进制文件

对于最新的 `bleeding edge` 构建：

```
docker run amphitheatre/amp:edge amp <command>
```

{{ end() }}

## 验证

安装 Amphitheatre 命令行工具后，请使用以下命令验证是否正确安装：

```
amp version
```

## 故障排除

如果在安装 Amphitheatre 命令行工具时遇到任何问题，请查看故障排除 FAQ 中的错误消息。
