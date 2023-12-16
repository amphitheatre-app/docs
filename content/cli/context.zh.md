+++
title = "amp context"
description = "配置访问多个集群。"
weight = 1
+++

上下文是一组集群访问参数。每个上下文包含其名称、服务器地址和访问凭据等信息。当前
上下文是任何 Amphitheatre CLI 命令的默认值。

## 可用命令
- [show](#amp-context-show)     打印当前上下文
- [list](#amp-context-list)     列出所有可用的上下文
- [use](#amp-context-use)       选择一个现有的上下文或创建一个新的
- [delete](#amp-context-delete) 删除一个上下文

> 使用 "amp <command> --help" 查看有关特定命令的更多信息。

## amp context show

打印当前上下文

### 用法
```
amp context show [选项]
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

### 示例

```
amp context show
```

```
Context {
    name: "default",
    url: "http://localhost:8170",
    token: "",
}
```

## amp context list

列出所有可用的上下文

### 用法
```
amp context list [选项]
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

### 示例

```
amp context list
```

```
Context {
    name: "default",
    url: "http://localhost:8170",
    token: "",
}
Context {
    name: "local",
    url: "http://api.amphitheatre.local:8170",
    token: "",
}
```

## amp context use

选择一个现有的上下文或创建一个新的

### 用法
```
amp context use [选项] [名称]
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

### 示例

#### 设置默认上下文。

```
amp context use
```

这将提示您选择一个现有的上下文或创建一个新的。

#### 您也可以指定一个名称：
```
amp context use local
```

## amp context delete

删除一个上下文

### 用法
```
amp context delete [选项] <名称>
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

### 示例

#### 删除 `local` 上下文

```
amp context delete local
```
