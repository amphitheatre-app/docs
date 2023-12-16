+++
title = "运行 Python 应用"
description = "学习如何在 Amphitheatre 上部署 Python 应用"
weight = 5
+++

在 Amphitheatre 上运行应用程序基本上就是找出如何将其打包为可部署的镜像。一旦打包
好，就可以部署到 Amphitheatre 平台。

## 示例应用程序

您可以从 [GitHub 存储库](https://github.com/amphitheatre-app/amp-example-go) 获
取示例的代码。只需运行 `git clone
https://github.com/amphitheatre-app/amp-example-go` 来获取本地副本。

`amp-example-python` 应用程序，正如您所期望的示例一样，非常小。它是一个使用
Flask web 框架的 Python 应用程序。以下是 `web.py` 中的所有代码：

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def index():
  return 'hello, world'
```

您将需要安装 Flask 本身，或者按照 [Flask 安装指
南](https://flask.palletsprojects.com/en/1.1.x/installation/#virtual-environments)
推荐的方式设置虚拟环境。

一旦激活了虚拟环境，运行：

```sh
python -m pip install -r requirements.txt
```

这将加载 Flask 和其他所需的包。其中一个包将是 gunicorn，它不是 Flask 的依赖项，
但在我们将应用程序部署到 Amphitheatre 时将被使用。

## 测试应用程序

Flask 应用程序使用 `flask run` 命令运行，但在运行之前，您需要设置一个名为
`FLASK_APP` 的环境变量，以指定要运行的应用程序。

```
$ FLASK_APP=hellofly flask run

* Serving Flask app "web"
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

这将运行我们的 Web 应用程序，您应该能够在本地连接到它的 `localhost:5000` 上。

现在，让我们继续将此应用程序部署到 Amphitheatre。

## 安装 Amphitheatre

我们已准备好开始使用 Amphitheatre，这意味着我们需要 `amp`，我们的 CLI 应用程序，
用于管理在 Amphitheatre 上的应用程序。如果您已经安装了它，请继续。如果没有，请转
到 [我们的安装指南](@/installation/_index.md)。

## 初始化 Character

要在 Amphitheatre 上启动应用程序，请在源代码所在的目录中运行 `amp init`。这将通
过检查您的源代码来为您创建和配置一个 `Character`，然后提示您部署。

```
$ amp init

Scanning source code
 Detected Python app
 Using the following build configuration
         Builder: paketobuildpacks/builder:base
         Buildpacks: gcr.io/paketo-buildpacks/python
Wrote config file .amp.toml
Your Character is ready. run with `amp run`
...
```

首先，此命令会扫描您的源代码，以确定如何构建部署镜像以及识别应用程序所需的任何其
他配置，如密钥和暴露的端口。

在扫描源代码并打印结果后，`amp` 会为您创建一个 `Character`，并将配置写入
`.amp.toml` 文件。然后，您将被提示构建和部署您的 character。

关于内置的 Python 构建器需要知道的一件事是，它将自动复制目录的内容到可部署的镜像
中。这是您可以移动静态资产，如模板和其他文件到您的应用程序的方式。另一件需要知道
的事情是，它使用 Procfile 来运行应用程序；Procfile 用于在其他平台上部署 Python
应用程序，因此我们保持了简单。Procfile 包含启动应用程序的说明。以下是我们的内
容：

```
web: gunicorn web:app
```

这表示应用程序的 Web 组件由 gunicorn 提供（我们在讨论依赖项时提到过）并且应该运
行我们为 Flask 设置的 Web Flask 应用程序。

完成后，您的应用程序将在 Amphitheatre 上运行。

## 在 .amp.toml 中

`.amp.toml` 文件现在包含了部署 `Character` 的默认配置。如果我们查看 `.amp.toml`
文件，我们可以在其中看到它：

```toml
name = "amp-example-python"
version = "0.0.1"
authors = ["Eguo Wang <wangeguo@gmail.com>"]
edition = "v1"
description = "一个简单的 Python 示例应用程序"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-python"
repository = "https://github.com/amphitheatre-app/amp-example-python"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "python", "getting-started"]
categories = ["example"]
```

amp 命令将始终引用当前目录中的此文件，特别是 Character 名称值在开头。该名称将用
于标识 Character 到 Amphitheatre 平台。文件的其余部分包含在部署 Character 时要应
用的设置。

## 部署到 Amphitheatre

要部署您的 `Character`，只需运行：

```sh
amp run
```

这将查找我们的 `.amp.toml` 文件，并从中获取 Character 名称
`amp-example-python`。然后，`amp` 将开始部署我们的 `Character` 到 Amphitheatre
平台。完成后，`amp` 会将您返回到命令行。

## 到达目的地

您已成功构建并部署了您的第一个 Python 应用程序到 Amphitheatre。
