+++
title = "amp config"
description = "与全局 Amphitheatre 配置文件交互"
weight = 1
+++



## 可用命令
- [find](#amp-config-find)      定位配置文件
- [list](#amp-config-list)      列出全局 Amphitheatre 配置中设置的所有值
- [set](#amp-config-set)        在全局 Amphitheatre 配置中设置值
- [unset](#amp-config-unset)    在全局 Amphitheatre 配置中取消设置值

> 使用 "amp <command> --help" 查看有关特定命令的更多信息。

## amp config find

定位配置文件


### 用法
```
amp config find [选项]
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

## amp config list

列出全局 Amphitheatre 配置中设置的所有值


### 用法
```
amp config list [选项]
```

### 选项

```
-a, --all              显示所有配置的值
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

### 环境变量

* `AMP_ALL`（与 `--all` 相同）

## amp config set

在全局 Amphitheatre 配置中设置值

### 用法
```
amp config set [选项] <KEY> <VALUE>
```

### 选项
```
  -g, --global           为全局配置设置值
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

### 示例:

#### 将注册表标记为不安全
```
amp config set insecure-registries <insecure1.io>
```

#### 全局设置默认镜像仓库
```
amp config set default-repo <myrepo>
```

#### 全局设置多级仓库支持
```
amp config set multi-level-repo true
```

### 环境变量

* `AMP_GLOBAL`（与 `--global` 相同）

## amp config unset

在全局 Amphitheatre 配置中取消设置值

### 用法
```
amp config unset [选项] <KEY>
```

### 选项
```
-g, --global           为全局配置取消设置值
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

### 环境变量

* `AMP_GLOBAL`（与 `--global` 相同）
