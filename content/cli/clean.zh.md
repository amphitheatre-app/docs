+++
title = "amp clean"
description = "删除由 Amphitheatre 部署的任何资源"
weight = 1
+++

## 用法
```
amp clean [选项] <ID>
```

## 参数：
```
  <ID>  要删除的 playbook 的 ID
```

## 选项
```
    --assume-yes <ASSUME_YES>  如果为 true，amp 将跳过用户的确认（是/否） [默认值：true] [可能的值：true, false]
    --dry-run                  如果为 true，amp 将跳过用户的确认（是/否）并默认为是
-f, --filename <FILENAME>      Amphitheatre 配置文件的路径或 URL [默认值：.amp.toml]
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

## 示例

#### 打印要删除的资源
```
amp clean --dry-run 0b59f0f4-9893-4635-9b72-ce92f320ddf1
```

## 环境变量

* `AMP_ASSUME_YES`（与 `--assume-yes` 相同）
* `AMP_DRY_RUN`（与 `--dry-run` 相同）
* `AMP_FILENAME`（与 `--filename` 相同）
