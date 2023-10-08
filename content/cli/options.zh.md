+++
title = "amp options"
description = "全局命令行选项列表（适用于所有命令）"
weight = 1
+++

## 用法
```
amp options
```

## 选项
以下选项可以传递给任何命令：

```
  -v, --verbose...             每次出现更多的输出
  -q, --quiet...               每次出现更少的输出
  -c, --config <CONFIG>        用于全局配置的文件 [环境变量: AMP_CONFIG=] [默认值: ~/.config/amphitheatre/config.toml]
      --interactive            允许用户提示以获取更多信息 [环境变量: AMP_INTERACTIVE=]
      --timestamps             在日志中打印时间戳 [环境变量: AMP_TIMESTAMPS=]
      --update-check           检查 Amphitheatre 的更新版本 [环境变量: AMP_UPDATE_CHECK=]
      --verbosity <VERBOSITY>  日志级别：其中之一 [panic fatal error warning info debug trace] [环境变量: AMP_VERBOSITY=] [默认值: warning]
  -h, --help                   打印帮助
```

## 环境变量

* `AMP_CONFIG`（与 `--config` 相同）
* `AMP_INTERACTIVE`（与 `--interactive` 相同）
* `AMP_TIMESTAMPS`（与 `--timestamps` 相同）
* `AMP_UPDATE_CHECK`（与 `--update-check` 相同）
* `AMP_VERBOSITY`（与 `--verbosity` 相同）
