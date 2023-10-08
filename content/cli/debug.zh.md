+++
title = "amp debug"
description = "以调试模式运行流水线"
weight = 1
+++

## 用法
```
amp debug [选项]
```

## 选项
```
    --assume-yes <ASSUME_YES>  如果为 true，amp 将跳过用户的确认（是/否） [默认值：true] [可能的值：true, false]
    --auto-build               当设置为 false 时，构建将等待 API 请求而不是自动运行
    --auto-create-config       如果为 true，amp 将尝试为用户的运行创建配置（如果找不到配置）
    --auto-deploy              当设置为 false 时，部署将等待 API 请求而不是自动运行
    --auto-sync                当设置为 false 时，同步将等待 API 请求而不是自动运行
    --cleanup                  在 dev 或 debug 模式被中断后删除部署
-f, --filename <FILENAME>      Amphitheatre 配置文件的路径或 URL [默认值：.amp.toml]
    --force                    如果需要，重新创建 Kubernetes 资源以进行部署，警告：可能会导致停机！
    --iterative-status-check   在每个部署步骤之后运行 `status-check`，而不是在所有部署结束后一起运行（默认）
-p, --profile <PROFILE>        通过名称激活配置文件（以 `-` 为前缀可禁用配置文件） [默认值：[]]
    --protocols <PROTOCOLS>    要支持的调试器协议的优先排序顺序 [默认值：[]]
    --status-check             等待部署的资源稳定
    --tail                     从部署的对象中流式传输日志
    --trigger <TRIGGER>        如何触发更改检测？（轮询、通知或手动） [默认值：notify]
```

> 使用 "amp options" 查看全局命令行选项列表（适用于所有命令）。

## 示例

#### 启动并流式传输部署对象的日志
```
amp debug --tail
```

## 环境变量

* `AMP_ASSUME_YES`（与 `--assume-yes` 相同）
* `AMP_AUTO_BUILD`（与 `--auto-build` 相同）
* `AMP_AUTO_CREATE_CONFIG`（与 `--auto-create-config` 相同）
* `AMP_AUTO_DEPLOY`（与 `--auto-deploy` 相同）
* `AMP_AUTO_SYNC`（与 `--auto-sync` 相同）
* `AMP_CLEANUP`（与 `--cleanup` 相同）
* `AMP_FILENAME`（与 `--filename` 相同）
* `AMP_FORCE`（与 `--force` 相同）
* `AMP_ITERATIVE_STATUS_CHECK`（与 `--iterative-status-check` 相同）
* `AMP_PROFILE`（与 `--profile` 相同）
* `AMP_PROTOCOLS`（与 `--protocols` 相同）
* `AMP_STATUS_CHECK`（与 `--status-check` 相同）
* `AMP_TAIL`（与 `--tail` 相同）
* `AMP_TRIGGER`（与 `--trigger` 相同）
