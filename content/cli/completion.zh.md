+++
title = "amp completion"
description = "为给定的Shell（bash或zsh）输出Shell自动完成"
weight = 1
+++

## 用法
```
amp completion [选项] <SHELL>
```

## 参数

* `<SHELL>`  可能的值：`bash`, `elvish`, `fish`, `powershell`, `zsh`

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

通过软件包管理器安装 Amphitheatre CLI 时，可能无需额外的 shell 配置即可获得自动完成支持。对于 Homebrew，请参阅 [https://docs.brew.sh/Shell-Completion](https://docs.brew.sh/Shell-Completion)

如果您需要手动设置自动完成，请按照以下说明进行操作。确切的配置文件位置可能因您的系统而异。在测试自动完成是否有效之前，请确保重新启动您的 shell。

## bash

首先，请确保使用您的软件包管理器安装 bash-completion。然后，将以下内容添加到您的 `~/.bash_profile`：

```
eval "$(amp completion bash)"
```

## zsh

生成一个 `_amp` 自动完成脚本，并将其放在您的 `$fpath` 的某个位置：

```
amp completion zsh > /usr/local/share/zsh/site-functions/_amp
```

确保以下内容存在于您的 `~/.zshrc`：

```
autoload -U compinit
compinit -i
```

建议使用 Zsh 版本 5.7 或更高版本。

## fish

生成一个 `amp.fish` 自动完成脚本：

```
amp completion fish > ~/.config/fish/completions/amp.fish
```

## PowerShell

使用以下命令打开您的配置文件脚本：

```
mkdir -Path (Split-Path -Parent $profile) -ErrorAction SilentlyContinue
notepad $profile
```

添加以下行并保存文件：

```
Invoke-Expression -Command $(amp completion powershell | Out-String)
```
