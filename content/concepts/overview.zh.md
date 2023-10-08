+++
title = "概述"
description = "介绍 Amphitheatre，包括其主要功能和工作流程。"
weight = 1
+++

## 功能

- 快速本地 Kubernetes开发

    - **优化的“源码到 Kubernetes”** - Amphitheatre 检测您的源代码变化，并处理**构建**、**推送**、**测试**和自动部署您的应用程序，使用**基于策略的镜像标记**和**高度优化的快速本地工作流程**
    - **持续反馈** - Amphitheatre 自动管理部署日志和资源端口转发

- Amphitheatre项目可以在任何地方使用

    - **与其他开发者共享** - Amphitheatre 是**与世界分享项目**的最简单方式：`git clone` 和 `amp run`
    - **上下文感知** - 使用 Amphitheatre 配置文件、本地用户配置、环境变量和标志轻松适应不同环境
    - **CI/CD 构建块** - 将 `amp build`、`amp test` 和 `amp deploy`作为 CI/CD 流水线的一部分，或者简单地使用 `amp run` 进行端到端开发
    - **GitOps 集成** - 使用 `amp render` 构建您的镜像并渲染模板化的 Kubernetes 清单，用于 GitOps 工作流程

- `.amp.toml` - 用于您的项目的单一可插拔的声明性配置

    - **amp init** - Amphitheatre 可以发现您的构建和部署配置，并生成 Amphitheatre 配置
    - **多组件应用** - Amphitheatre 支持具有多个组件的应用程序，非常适用于基于微服务的应用程序
    - **自带工具** - Amphitheatre 具有可插拔的架构，允许不同的构建和部署阶段的不同实现

- 轻量级

    - **最小化的流水线** - Amphitheatre 提供了一套有见地的、最小化的流水线，以保持简洁

## 工作流程

Amphitheatre 通过将常见的开发阶段组织成一个简单的命令来简化您的开发工作流程。每次运行 `amp dev` 时，系统会执行以下操作：

1. 收集并监视您的源代码的变化
2. 如果用户将文件标记为可同步，则直接将文件同步到 Pod
3. 从源代码构建构件
4. 使用[container-structure-tests](https://github.com/GoogleContainerTools/container-structure-test)或自定义脚本测试构建的构件
5. 对构件进行标记
6. 推送构件
7. 部署构件
8. 监视已部署的构件
9. 在退出时（Ctrl+C）清理已部署的构件

> **注意**\
可以跳过这些阶段中的任何一个。

Amphitheatre 还会自动为您管理以下实用工具：

- 使用 `kubectl port-forward` 将部署的资源端口转发到您的本地计算机
- 从已部署的 Pod 进行日志聚合
