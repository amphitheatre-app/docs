+++
title = "介绍"
description = "欢迎访问 Amphitheatre 文档站点！"
+++

> **重要提示!!!**\
本文档仍在进行中，内容将随时更新，一些链接或指南是预览版，不代表实际可用性，请勿用于正式生产环境中。

## 什么是 Amphitheatre?

**Amphitheatre** 是一个开源的开发者平台，旨在帮助开发者在云端立即启动新的自动化开发环境。它提供按需且预先配置好的所有工具、库和依赖项，以确保您能够立即开始编写代码。您可以在本地编辑应用程序源代码，Amphitheatre 会自动将您的变更增量部署到 Kubernetes 集群，使得在本地开发和远程部署之间的切换变得更加顺畅和高效。

## 产品特性

- **无依赖**：无需配置本地开发环境，支持多种编程语言和框架。适用于 29+ 种编程语言，以及 400+ 技术栈。基于标准规范轻松搞定企业开发所需的自定义需求。

- **轻松创建无限量的隔离集成测试环境**：快速生成无限数量的隔离集成测试环境，无限制地简化测试流程并降低成本。
- **支持微服务架构体系和多人协作联调测试**：支持具有多个组件的应用程序，非常适用于基于微服务的应用程序和协作多人开发。
- **本地开发实时部署到远程集群**：可让您完全跳过镜像构建，它使用新代码更新正在运行的容器，只需几秒钟而不是几分钟。即使是编译语言或更改依赖项，「实时更新」也是快速且可靠的。
- **可插拔的生态应用市场，全方位提升效率**：集成开发、测试、部署和其他工具，创建全面的软件集成解决方案，自动化工作流程以简化软件开发。
- **交互式运行实例快照**：快照可让您共享您的开发环境并在问题上进行协作，就像查看您旁边的监视器一样快。并以交互方式及时探索该时刻的日志和错误状态。这可以帮助进行异步调试，并为错误报告添加上下文。

## 使用 Amphitheatre 开始开发

{{ grid(columns=3)}}
{{ column() }}{{ card(title="入门指南", text="如何在您的环境中快速上手并运行 Amphitheatre。", url="/zh/getting-started/") }}{{ end() }}
{{ column() }}{{ card(title="核心概念", text="了解 Amphitheatre，包括其主要功能和能力。", url="/zh/concepts/") }} {{ end() }}
{{ column() }}{{ card(title="安装指南", text="了解如何在您的基础架构中安装 Amphitheatre。", url="/zh/installation/") }} {{ end() }}
{{ end() }}

## 语言与框架指南

{{ grid(columns=3)}}
{{ column() }}{{ card(title="Golang", text="学习如何部署 Go 应用程序。", url="/zh/examples/golang/") }}{{ end() }}
{{ column() }}{{ card(title="Python", text="学习如何部署 Python 应用程序。", url="/zh/examples/python/") }}{{ end() }}
{{ column() }}{{ card(title="Java", text="学习如何部署 Java 应用程序。", url="/zh/examples/java/") }}{{ end() }}
{{ column() }}{{ card(title="NodeJs", text="学习如何部署 NodeJs 应用程序。", url="/zh/examples/nodejs") }}{{ end() }}
{{ column() }}{{ card(title="Rust", text="学习如何部署 Rust 应用程序。", url="/zh/examples/rust") }}{{ end() }}
{{ column() }}{{ card(title="PHP", text="学习如何部署 PHP 应用程序。", url="/zh/examples/php") }}{{ end() }}
{{ end() }}

## 附加信息

{{ grid(columns=2)}}
{{ column() }}{{ card(title="参考资料", text="关于 Amphitheatre API、CLI 等详细文档。", url="/zh/references/") }}{{ end() }}
{{ column() }}{{ card(title="贡献指南", text="如何为 Amphitheatre 项目以及各个代码库做出贡献。", url="/zh/contributing/") }} {{ end() }}
{{ end() }}
