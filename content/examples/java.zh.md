+++
title = "运行 Java 应用"
description = "学习如何在 Amphitheatre 上部署 Java 应用"
weight = 2
+++

在 Amphitheatre 上运行应用程序基本上就是找出如何将其打包为可部署的镜像。一旦打包好，就可以部署到 Amphitheatre 平台。

## 示例应用程序

我们的示例将是一个使用 Java 和 [Spring Boot](https://spring.io/projects/spring-boot) 的基本 "Hello World" 示例。

您可以从 [GitHub 存储库](https://github.com/amphitheatre-app/amp-example-java) 获取示例的代码。只需运行 `git clone https://github.com/amphitheatre-app/amp-example-java` 来获取本地副本。

### 创建一个简单的 Web 应用程序

现在，您可以为一个简单的 Web 应用程序创建一个 Web 控制器，如下面的代码（来自 `src/main/java/hello/HelloController.java`）所示：

```java
package hello;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;

@RestController
public class HelloController {
    @RequestMapping("/")
    public String index() {
        return "Hello, World!";
    }
}
```

### 创建一个应用程序类

Spring Initializr 会为您创建一个简单的应用程序类。然而，在这种情况下，它太简单了。您需要修改应用程序类以匹配以下代码（来自 `src/main/java/hello/Application.java`）：

```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## 运行应用程序

要运行应用程序，请在终端窗口（在 `complete` 目录中）中运行以下命令：

```
./mvnw spring-boot:run
```

现在，在另一个终端窗口中使用 curl 运行服务，运行以下命令（显示其输出）：

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
 Detected Java app
 Using the following build configuration
         Builder: paketobuildpacks/builder:base
		 Buildpacks: gcr.io/paketo-buildpacks/java
Wrote config file .amp.toml
Your Character is ready. run with `amp run`
...
```

首先，此命令会扫描您的源代码，以确定如何构建部署镜像，以及识别应用程序所需的任何其他配置，如密钥和暴露的端口。

在扫描源代码并打印结果后，`amp` 会为您创建一个 `Character`，并将配置写入 `.amp.toml` 文件。然后，您将被提示构建和部署您的角色。一旦完成，您的应用程序将在 Amphitheatre 上运行。

## 在 .amp.toml 中

`.amp.toml` 文件现在包含了部署您的 `Character` 的默认配置。如果我们查看 `.amp.toml` 文件，我们可以在其中看到它：

```toml
name = "amp-example-java"
version = "0.0.1"
authors = ["Eguo Wang <wangeguo@gmail.com>"]
edition = "v1"
description = "一个简单的 Java 示例应用程序"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-java"
repository = "https://github.com/amphitheatre-app/amp-example-java"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "java", "getting-started"]
categories = ["example"]
```

`amp` 命令将始终引用当前目录中的此文件（如果存在），特别是以开始的角色名称值。该名称将用于标识 Amphitheatre 平台上的角色。文件的其余部分包含部署角色时要应用的设置。

有关更多选项，请参阅 [Paketo Java Buildpack 文档](https://paketo.io/docs/howto/java/)。

## 部署到 Amphitheatre

要部署您的 `Character`，只需运行：

```sh
amp run
```

这将查找我们的 `.amp.toml` 文件，并从中获取 Character 名称 `amp-example-java`。然后，`amp` 将开始部署我们的 `Character` 到 Amphitheatre 平台。完成后，`amp` 会将您返回到命令行。

## 到达目的地

您已成功构建并部署了您的第一个 Java 应用程序到 Amphitheatre。
