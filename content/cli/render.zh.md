+++
title = "amp render"
description = "执行所有镜像构建操作，并输出渲染的 Kubernetes 清单"
weight = 1
+++


## 用法
```
amp render [选项]
```

## 选项

```
    --assume-yes <ASSUME_YES>  如果为 true，amp 将跳过用户的确认（是/否） [默认值：true] [可能的值：true, false]
-f, --filename <FILENAME>      Amphitheatre 配置文件的路径或 URL [默认值：.amp.toml]
    --loud                     显示构建日志和输出
-o, --output <OUTPUT>          要写入更改的配置文件的文件（而不是标准输出）
-p, --profile <PROFILE>        通过名称激活配置文件（以 `-` 为前缀可禁用配置文件） [默认值：[]]
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

## 环境变量

* `AMP_ASSUME_YES`（与 `--assume-yes` 相同）
* `AMP_FILENAME`（与 `--filename` 相同）
* `AMP_LOUD`（与 `--loud` 相同）
* `AMP_OUTPUT`（与 `--output` 相同）
* `AMP_PROFILE`（与 `--profile` 相同）
