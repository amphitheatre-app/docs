+++
title = "运行 Golang 应用"
description = "学习如何在 Amphitheatre 上部署 Golang 应用程序"
weight = 1
+++

在 Amphitheatre 上运行应用程序实际上是找出如何将其打包成可部署的镜像。一旦打包完成，就可以将其部署到 Amphitheatre 平台。

## 示例应用程序

您可以从 [GitHub 存储库](https://github.com/amphitheatre-app/amp-example-go) 获取示例的代码。只需运行以下命令以获取本地副本：`git clone https://github.com/amphitheatre-app/amp-example-go`。

`amp-example-go` 应用程序是一个小型示例，正如您所期望的那样。它是一个 Go 应用程序，会循环打印 'Hello world'。以下是 `main.go` 中的所有代码：

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	for {
		fmt.Println("Hello world!")
		time.Sleep(time.Second * 1)
	}
}
```

## 构建应用程序

与大多数 Go 应用程序一样，简单的 `go build` 将创建一个可运行的二进制文件。因此，原始应用程序可以正常运行。现在，将其打包以供 Amphitheatre 使用。

## 安装 Amphitheatre

我们已准备好开始使用 Amphitheatre，并且这意味着我们需要 `amp`，这是用于管理 Amphitheatre 上应用程序的 CLI 应用程序。如果您已经安装了它，请继续。如果没有，请跳转到 [我们的安装指南](@/installation/_index.zh.md)。

## 初始化 Character

要在 Amphitheatre 上启动应用程序，请在包含源代码的目录中运行 `amp init`。这将通过检查您的源代码为您创建和配置一个 `Character`，然后提示您进行部署。

```
$ amp init

Scanning source code
 Detected Go app
 Using the following build configuration
         Builder: paketobuildpacks/builder:base
         Buildpacks: gcr.io/paketo-buildpacks/go
Wrote config file .amp.toml
Your Character is ready. run with `amp run`
...
```

首先，此命令会扫描您的源代码以确定如何构建部署映像，以及识别应用程序所需的任何其他配置，例如密钥和暴露的端口。

扫描源代码并打印结果后，`amp` 会为您创建一个 `Character`，并将您的配置写入一个 `.amp.toml` 文件。然后，您将被提示构建和部署您的 Character。一旦完成，您的应用程序将在 Amphitheatre 上运行。

## 在 .amp.toml 中

`.amp.toml` 文件现在包含用于部署您的 `Character` 的默认配置。如果我们查看 `.amp.toml` 文件，可以在其中看到：

```toml
name = "amp-example-go"
version = "0.0.2"
authors = ["Eguo Wang <wangeguo@gmail.com>"]
edition = "v1"
description = "A simple Golang example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-go"
repository = "https://github.com/amphitheatre-app/amp-example-go"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "golang", "getting-started"]
categories = ["example"]
```

如果该文件存在，amp 命令将始终引用当前目录中的此文件，特别是开始时的 Character 名称值。该名称将用于标识 Character 到 Amphitheatre 平台。文件的其余部分包含部署 Character 时要应用的设置。

有关更多选项，请参阅 [Paketo Go Buildpack 文档](https://paketo.io/docs/howto/go/)。

## 部署到 Amphitheatre

要部署您的 Character，请运行：

```sh
amp run
```

这将查找我们的 `.amp.toml` 文件，并从中获取 Character 名称 `amp-example-go`。然后，`amp` 将开始部署 Character 到 Amphitheatre 平台。`amp` 将在完成时返回到命令
