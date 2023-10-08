+++
title = "运行 PHP 应用"
description = "学习如何在 Amphitheatre 上部署 PHP 应用"
weight = 4
+++

在 Amphitheatre 上运行应用程序基本上就是找出如何将其打包为可部署的镜像。一旦打包好，就可以部署到 Amphitheatre 平台。

## 示例应用程序

您可以从 [GitHub 存储库](https://github.com/amphitheatre-app/amp-example-php) 获取示例的代码。只需运行 `git clone https://github.com/amphitheatre-app/amp-example-php` 来获取本地副本。

`amp-example-php` 应用程序，正如您所期望的示例一样，非常小。它是一个 PHP 应用程序，打印出 'Hello World'。以下是 `public_html/index.php` 中的所有代码：

```php
<?php
$message = "Hello World!";
echo($message);
```

## 运行应用程序

```sh
cd ~/public_html
php -S localhost:8080
```

然后使用 curl 运行服务（在单独的终端窗口中），通过运行以下命令（附带其输出）：

```
$ curl localhost:8080
Hello, World!
```

## 安装 Amphitheatre

我们已准备好开始使用 Amphitheatre，这意味着我们需要 `amp`，我们的 CLI 应用程序，用于管理在 Amphitheatre 上的应用程序。如果您已经安装了它，请继续。如果没有，请转到 [我们的安装指南](@/installation/_index.md)。

## 初始化 Character

要在 Amphitheatre 上启动应用程序，请在源代码所在的目录中运行 `amp init`。这将通过检查您的源代码来为您创建和配置一个 `Character`，然后提示您部署。

```
$ amp init

Scanning source code
 Detected PHP app
 Using the following build configuration
         Builder: paketobuildpacks/builder:full
         Buildpacks: paketo-buildpacks/php
Wrote config file .amp.toml
Your Character is ready. run with `amp run`
...
```

首先，此命令会扫描您的源代码，以确定如何构建部署镜像以及识别应用程序所需的任何其他配置，如密钥和暴露的端口。

在扫描源代码并打印结果后，`amp` 会为您创建一个 `Character`，并将配置写入 `.amp.toml` 文件。然后，您将被提示构建和部署您的 character。一旦完成，您的应用程序将在 Amphitheatre 上运行。

## 在 .amp.toml 中

`.amp.toml` 文件现在包含了部署您的 `Character` 的默认配置。如果我们查看 `.amp.toml` 文件，我们可以在其中看到它：

```toml
name = "amp-example-php"
version = "0.0.1"
authors = ["Eguo Wang <wangeguo@gmail.com>"]
edition = "v1"
description = "一个简单的 PHP 示例应用程序"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-php"
repository = "https://github.com/amphitheatre-app/amp-example-php"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "php", "getting-started"]
categories = ["example"]

[[build.env]]
name = "BP_PHP_WEB_DIR"
value = "public_html"
```

`amp` 命令将始终引用当前目录中的此文件（如果存在），特别是以开始的角色名称值。该名称将用于标识 Amphitheatre 平台上的角色。文件的其余部分包含部署角色时要应用的设置。

请参阅 [Paketo PHP Buildpack
文档](https://paketo.io/docs/howto/php/)
以获取更多选项。

## 部署到 Amphitheatre

要部署您的 `Character`，只需运行：

```sh
amp run
```

这将查找我们的 `.amp.toml` 文件，并从中获取 Character 名称 `amp-example-php`。然后，`amp` 将开始部署我们的 `Character` 到 Amphitheatre 平台。完成后，`amp` 会将您返回到命令行。

## 到达目的地

您已成功构建并部署了您的第一个 PHP 应用程序到 Amphitheatre。
