+++
title = "安装桌面应用程序"
description = "安装 Amphitheatre 桌面应用程序。"
weight = 3
+++

Amphitheatre 桌面是轻量且易于在所有主要平台上安装的应用程序。您可以在 [Amphitheatre
网站](https://amphitheatre.app/) 上找到所有安装选项。

## 系统要求

在继续 Amphitheatre 桌面的安装之前，请验证您的系统是否符合系统要求。

### 硬件要求

最低硬件要求：

- 2 GHz 或更快的处理器
- 1 GB RAM
- 1 GB 磁盘空间

### 平台

Amphitheatre 桌面已在以下平台上进行了测试：macOS、Windows 和 Linux。

尽管 Amphitheatre 桌面可能仍可在操作系统的维护结束日期之后继续工作，但我们强烈建议使用支持的操作系统版本。请参阅支持的操作系统版本列表。

{{ tabs(id="os", items=["macOS", "Windows", "Linux"])}}

{{ pane(tab="os", index=1) }}

- 从 [Amphitheatre
  网站](https://amphitheatre.app/) 下载 macOS 版的 Amphitheatre 桌面。
- 双击 amphitheatre-desktop-{version}.dmg 并将 amphitheatre-desktop.app 拖动到 Applications 文件夹中，使其在 macOS Launchpad 中可用。
- 通过右键单击图标以打开上下文菜单并选择选项，将 Amphitheatre 桌面添加到 Dock。

{{ end() }}

{{ pane(tab="os", index=2) }}

- 从 [Amphitheatre 桌面安装程序](https://amphitheatre.app/) 下载 Windows 版。
- 运行 Amphitheatre-Desktop-Setup-{version}.exe 安装程序来安装 Amphitheatre 桌面。默认情况下，Amphitheatre 桌面安装在 `C:\users\{username}\AppData\Local\Programs\AmphitheatreDesktop` 下。

{{ end() }}

{{ pane(tab="os", index=3) }}

Linux 用户有以下下载选项：`.rpm` 和 `.deb` 文件，`AppImage` 文件以及 `.snap` 文件。

#### 默认包格式

以下示例展示了如何在 Ubuntu 20.04 上安装 Amphitheatre 桌面：

- 从此处下载 .deb 文件：
  [https://repo.amphitheatre.app/desktop/releases/latest/amphitheatre-desktop.deb](https://repo.amphitheatre.app/desktop/releases/latest/amphitheatre-desktop.deb)。
- 在 Linux 命令行中，输入以下命令：

    ```
    sudo apt install <path-to-deb-file>
    ```

    您还可以使用一种用于 `.deb` 文件安装的应用程序。
    > **注意**\
    对于基于 RPM 的发行版，流程类似，只是终端命令有轻微的不同。

#### AppImage

- 从
  [https://repo.amphitheatre.app/desktop/releases/latest/amphitheatre-desktop.AppImage](https://repo.amphitheatre.app/desktop/releases/latest/amphitheatre-desktop.AppImage)
  下载 AppImage 文件。
- 转到您下载文件的目录，右键单击 AppImage 文件，然后选择 Properties > Permissions。
- 选择 Allow executing file as a program 复选框。

    > **注意**\
    一些文件管理器可能提供取消选择 Is executable 复选框或其他类似操作的选项。

- 在 Linux 命令行中，导航到您下载文件的目录，然后输入以下命令：

    ```sh
    chmod +x <file-name.AppImage>
    ./<file-name.AppImage>
    ```

在手动安装 Amphitheatre 桌面（而不是使用包管理器文件，如 .deb 或 .rpm）后，需要执行以下操作以允许协议处理。这假定您的 Linux 发行版使用 xdg-open 和 xdg-* 程序集来确定哪个应用程序可以处理自定义 URI。

- 在以下路径之一创建一个名为 `amphitheatre-desktop.desktop` 的文件：

- `~/.local/share/applications/`：为所有用户安装

- `/usr/share/applications`：为您的用户安装

- 该文件应具有以下内容，其中 `<path/to/executable>` 是您已安装解压后的 Amphitheatre 桌面可执行文件的绝对路径：

    ```
    [Desktop Entry]
    Name=Amphitheatre Desktop
    Exec=<path/to/executable> %U
    Terminal=false
    Type=Application
    Icon=logo
    StartupWMClass=AmphitheatreDesktop
    Comment=Amphitheatre Desktop - 开源 GUI 应用程序，可让您与 Amphitheatre 互动
    MimeType=x-scheme-handler/amp;
    Categories=Network;
    ```

- 然后运行以下命令：

    ```
    xdg-settings set default-url-scheme-handler amp amphitheatre-desktop.desktop
    ```

- 如果成功（以代码 0 退出），则您的 Amphitheatre 桌面安装应已设置为处理 `amp://` URI。

#### Snap

- 从 Snap 获取 [Amphitheatre 桌面包](https://amphitheatre.app/)。
- 在 Linux 命令行中，输入以下命令：

    ```
    sudo snap install amphitheatre-desktop-{version}.amd64.snap --dangerous --classic
    ```

{{ end() }}

## 更新

Amphitheatre 桌面支持自动更新。当有新版本可用时，应用程序会显示通知。

> **注意**\
自动更新仅适用于 exe、dmg 和 AppImage 安装。对其他发行版进行手动更新。

请查看 Amphitheatre 桌面的 [发布说明](https://github.com/amphitheatre-app/desktop/releases) 以获取有关功能、维护等信息。

## 备份和恢复数据

当升级到主要版本的 Amphitheatre 桌面时，用户数据，例如集群、偏好设置等，会迁移到新的数据结构。但我们建议您备份用户数据，这些数据通常存储在以下位置：

- macOS：`~/Library/Application Support/AmphitheatreDesktop/`
- Windows：`%APPDATA%\AmphitheatreDesktop\`
- Linux：`~/.config/AmphitheatreDesktop/` 或 `$XDG_CONFIG_HOME/AmphitheatreDesktop`

## 故障排除

如果在安装 Amphitheatre 桌面时遇到任何问题，请查看故障排除 FAQ 中的错误消息和其 [Github 问题](https://github.com/amphitheatre-app/desktop/issues)。
