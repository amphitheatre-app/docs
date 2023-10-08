+++
title = "amp diagnose"
description = "运行 Amphitheatre 的诊断工具"
weight = 1
+++


## 用法
```
amp diagnose [选项]
```

## 选项
```
    --assume-yes <ASSUME_YES>  如果为 true，amp 将跳过用户的确认（是/否） [默认值：true] [可能的值：true, false]
-f, --filename <FILENAME>      Amphitheatre 配置文件的路径或 URL [默认值：.amp.toml]
-p, --profile <PROFILE>        通过名称激活配置文件（以 `-` 为前缀可禁用配置文件） [默认值：[]]
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

## 示例
#### 搜索配置问题并打印生效的配置
```
amp diagnose
```

#### 打印给定配置文件的配置
```
amp diagnose --profile PROFILE
```

## 环境变量

* `AMP_ASSUME_YES`（与 `--assume-yes` 相同）
* `AMP_FILENAME`（与 `--filename` 相同）
* `AMP_PROFILE`（与 `--profile` 相同）
