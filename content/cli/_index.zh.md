+++
title = "CLI 手册"
weight = 60
sort_by = "weight"

[extra]
listing = false

[extra.sidebar]
linkable = true
+++

Amphitheatre 命令行界面提供以下命令：

全流程管道：

* [amp run](@/cli/run.zh.md) - 构建和部署一次
* [amp dev](@/cli/dev.zh.md) - 触发监视循环的构建和部署工作流程，退出时清理
* [amp debug](@/cli/debug.zh.md) - 以调试模式运行管道

用于 CI/CD 的管道构建块：

* [amp deploy](@/cli/deploy.zh.md) - 部署给定的镜像
* [amp clean](@/cli/clean.zh.md) - 清理已部署的工件
* [amp render](@/cli/render.zh.md) - 构建和标记镜像，并输出模板化的 Kubernetes 清单

开始一个新项目：

* [amp init](@/cli/init.zh.md) - 初始化 Amphitheatre 配置

其他命令：

* amp help - 打印帮助信息
* [amp version](@/cli/version.zh.md) - 获取 Amphitheatre 版本
* [amp completion](@/cli/completion.zh.md) - 设置 CLI 的标签完成
* [amp config](@/cli/config.zh.md) - 与全局配置文件交互
* [amp context](@/cli/context.zh.md) - 配置访问多个集群
* [amp diagnose](@/cli/diagnose.zh.md) - 诊断 Amphitheatre 在您的项目中的工作情况
* [amp list](@/cli/list.zh.md) - 列出所有正在运行的实例

## 全局标志

```
-v    --verbose           更多输出每次出现
-q    --quiet             更少输出每次出现
-c    --config            全局配置文件
      --interactive       允许用户提示获取更多信息
      --timestamps        在日志中打印时间戳
      --update-check      检查 Amphitheatre 的更新版本
      --verbosity         日志级别: 从 [panic fatal error warning info debug trace] 中选择之一
```

## 全局环境变量

* `AMP_CONFIG` (同 `--config`)
* `AMP_INTERACTIVE` (同 `--interactive`)
* `AMP_TIMESTAMPS` (同 `--timestamps`)
* `AMP_UPDATE_CHECK` (同 `--update-check`)
* `AMP_VERBOSITY` (同 `--verbosity`)
