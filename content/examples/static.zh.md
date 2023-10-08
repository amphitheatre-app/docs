+++
title = "运行静态网站"
description = "学习如何在 Amphitheatre 上部署静态网站"
weight = 8
+++

在 Amphitheatre 上运行应用程序基本上就是找出如何将其打包为可部署的镜像。一旦打包好，就可以部署到 Amphitheatre 平台。

老实说，静态网站不是一个应用程序。所以，我们实际上是在谈论部署一个用于提供一些静态内容的应用程序。

在此演示中，我们将使用 [goStatic](https://github.com/PierreZ/goStatic)，这是一个用 Go 编写的微型 Web 服务器，允许我们只需非常少的配置就可以提供静态文件。我们将提供一个 Dockerfile 和我们的内容，以便 Amphitheatre 可以将其转化为在虚拟机中运行的 Web 服务器。

## 示例应用程序

您可以从 [GitHub 存储库](https://github.com/amphitheatre-app/amp-example-static) 获取示例的代码。只需运行 `git clone https://github.com/amphitheatre-app/amp-example-static` 来获取本地副本。

或者，您可以按照本指南的步骤手动创建所有文件。

## 组装应用程序

到目前为止，如果您已经有了 `amp-example-static` 存储库的本地克隆，那么可以直接从其根目录运行 `amp run`，无需进一步操作即可部署静态站点。但这不会很有启发性。让我们看看示例存储库中包含的内容以及原因。

如果克隆了存储库，您的新应用程序已经有了自己的目录。否则，请创建一个。这不仅仅是为了整洁，或者为了让 `amp` 通过工作目录中的 `.amp.toml` 检测到您的应用程序（尽管这些都是很好的原因）。它还确保在构建 Docker 镜像时不会包含任何额外的文件 [build context](https://docs.docker.com/engine/reference/commandline/build/)。

我们将在此目录中完成所有操作：

```sh
cd amp-example-static
```

### 网站

我们的示例将是一个简单的静态网站。这可以是一个简单的 `index.html` 文件。让我们通过编写两个 HTML 文件并相互链接它们使其稍微复杂一些。

将这些 HTML 文件放入一个名为 `public` 的子目录中，该目录中的文件是我们的 `goStatic` 服务器将提供的文件。如果需要，请创建 `amp-example-static/public` 目录。

以下是 `index.html`，它是登陆页面：

```html
<html>
  <head>
    <title>
      来自 Amphitheatre 的问候
    </title>
  </head>
  <body>
    <h1>来自 Amphitheatre 的问候，这是一个静态网站</h1>
      <p>或者 <a href="goodbye.html">再见。</a></p>
  </body>
</html>
```

以下是 `goodbye.html`。

```html
<html>
  <head>
    <title>
      仍然来自 Amphitheatre 的问候
    </title>
  </head>
  <body>
    <h1>你说再见</h1>
      <p>但我说 <a href="index.html">你好。</a></p>
  </body>
</html>
```

### Dockerfile

`goStatic` 是设计为在容器中运行的，[镜像](https://hub.docker.com/r/pierrezemb/gostatic) 可在 Docker Hub 上找到。这对我们来说非常方便，因为 Amphitheatre 应用程序也需要容器镜像！

我们可以使用 goStatic 镜像作为基础镜像。我们只需将站点文件复制到镜像中的 /srv/http/。

以下是我们的 Dockerfile：

```Dockerfile
FROM pierrezemb/gostatic
COPY ./public/ /srv/http/
```

Dockerfile 应该放在工作目录中（在这里是 `amp-example-static`）。

## 安装 Amphitheatre

我们已准备好开始使用 Amphitheatre，这意味着我们需要 `amp`，我们的 CLI 应用程序，用于管理在 Amphitheatre 上的应用程序。如果您已经安装了它，请继续。如果没有，请转到 [我们的安装指南](@/installation/_index.md)。

## 初始化 Character

要在 Amphitheatre 上启动应用程序，请在源代码所在的目录中运行 `amp init`。这将通过检查您的源代码为您创建和配置一个 `Character`，然后提示您部署。

```sh
$ amp init

Scanning source code
 Detected a Dockerfile app
Wrote config file .amp.toml
Your Character is ready. run with `amp run`
...
```

首先，此命令会扫描您的源代码，以确定如何构建部署镜像以及识别应用程序所需的任何其他配置，如密钥和暴露的端口。

在扫描源代码并打印结果后，`amp` 会为您创建一个 `Character`，并将配置写入 `.amp.toml` 文件。然后，您将被提示构建和部署您的 character。完成后，您的应用程序将在 Amphitheatre 上运行。

## 在 .amp.toml 中

`.amp.toml` 文件现在包含了部署 `Character` 的默认配置。如果我们查看 `.amp.toml` 文件，我们可以在其中看到它：

```toml
name = "amp-example-static"
version = "0.0.1"
authors = ["Eguo Wang <wangeguo@gmail.com>"]
edition = "v1"
description = "一个静态网站"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-static"
repository = "https://github.com/amphitheatre-app/amp-example-static"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "static", "getting-started"]
categories = ["example"]

[build]
dockerfile = "Dockerfile"
```

amp 命令将始终引用当前目录中的此文件，特别是 Character 名称值在开头。该名称将用于标识 Character 到 Amphitheatre 平台。文件的其余部分包含在部署 Character 时要应用的设置。

## 部署到 Amphitheatre

要部署您的 `Character`，只需运行：

```sh
amp run
```

这将查找我们的 `.amp.toml` 文件，并从中获取 Character 名称 `amp-example-static`。然后，`amp` 将开始部署我们的 `Character` 到 Amphitheatre 平台。完成后，`amp` 会将您返回到命令行。

## 到达目的地

您已成功构建并部署了静态网站到 Amphitheatre。
