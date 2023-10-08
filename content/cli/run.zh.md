+++
title = "amp run"
description = "运行一条流水线，构建并部署一次"
weight = 1
+++

## 用法
```
amp run [选项]
```

## 选项

```
    --assume-yes <ASSUME_YES>  如果为 true，amp 将跳过用户的确认（是/否） [默认值：true] [可能的值：true, false]
    --auto-create-config       如果为 true，amp 将尝试为用户的运行创建配置，如果找不到配置文件的话
    --cleanup                  显示构建日志和输出
-f, --filename <FILENAME>      Amphitheatre 配置文件的路径或 URL [默认值：.amp.toml]
    --force                    如果必要，重新创建 Kubernetes 资源以进行部署，警告：可能会导致停机！
    --iterative-status-check   在每个部署步骤之后迭代运行 `status-check`，而不是在所有部署结束时一起运行（默认）
-p, --profile <PROFILE>        通过名称激活配置文件（以 `-` 为前缀可禁用配置文件） [默认值：[]]
    --status-check             等待已部署的资源稳定
    --tail                     从已部署对象流式传输日志
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

## 示例

#### 构建、测试、部署和查看日志
```
amp run --tail
```

#### 使用给定的配置文件运行
```
amp run -p <profile>
```

## 环境变量

* `AMP_ASSUME_YES`（与 `--assume-yes` 相同）
* `AMP_AUTO_CREATE_CONFIG`（与 `--auto-create-config` 相同）
* `AMP_CLEANUP`（与 `--cleanup` 相同）
* `AMP_FILENAME`（与 `--filename` 相同）
* `AMP_FORCE`（与 `--force` 相同）
* `AMP_ITERATIVE_STATUS_CHECK`（与 `--iterative-status-check` 相同）
* `AMP_PROFILE`（与 `--profile` 相同）
* `AMP_STATUS_CHECK`（与 `--status-check` 相同）
* `AMP_TAIL`（与 `--tail` 相同）
