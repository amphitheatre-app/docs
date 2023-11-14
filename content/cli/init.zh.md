+++
title = "amp init"
description = "在现有目录中创建 Amphitheatre 配置"
weight = 1
+++


## 用法
```
amp init [选项]
```

## 选项
```
    --assume-yes <ASSUME_YES>  如果为 true，amp 将跳过用户的确认（是/否） [默认值：true] [可能的值：true, false]
-f, --filename <FILENAME>      要将生成的清单配置写入的文件[默认值：.amp.toml]
    --force                    强制构建 Amphitheatre 配置
    --name <NAME>              指定配置名字. 默认为目录名
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

## 环境变量

* `AMP_ASSUME_YES`（与 `--assume-yes` 相同）
* `AMP_FILENAME`（与 `--filename` 相同）
* `AMP_PROFILE`（与 `--profile` 相同）
