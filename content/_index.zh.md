+++
title = "介绍"
description = "欢迎访问 Amphitheatre 文档站点！"
+++

> **重要提示!!!**\
本文档仍在进行中，内容将随时更新，一些链接或指南是预览版，不代表实际可用性，请勿用于正式生产环境中。

## 什么是 Amphitheatre?

**Amphitheatre** 是一个开源的开发者平台，旨在帮助开发者在云端立即启动新的自动化开发环境。它提供按需且预先配置好的所有工具、库和依赖项，以确保您能够立即开始编写代码。您可以在本地编辑应用程序源代码，Amphitheatre 会自动将您的变更增量部署到 Kubernetes 集群，使得在本地开发和远程部署之间的切换变得更加顺畅和高效。

## 使用 Amphitheatre 开始开发

{{ grid(columns=3)}}
{{ column() }}{{ card(title="入门指南", text="如何在您的环境中快速上手并运行 Amphitheatre。", url="/zh/getting-started/") }}{{ end() }}
{{ column() }}{{ card(title="快速入门", text="包含代码示例的教程集合，帮助您快速入门。", url="/zh/getting-started/quickstart/") }} {{ end() }}
{{ column() }}{{ card(title="概念", text="了解 Amphitheatre，包括其主要功能和能力。", url="/zh/concepts/") }} {{ end() }}
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
{{ column() }}{{ card(title="贡献", text="如何为 Amphitheatre 项目以及各个代码库做出贡献。", url="/zh/contributing/") }} {{ end() }}
{{ end() }}
