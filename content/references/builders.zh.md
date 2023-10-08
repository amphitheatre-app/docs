+++
title = "构建器"
weight = 2
+++

当您在 Amphitheatre 上部署应用程序时，该应用程序必须被组装成可部署的镜像。这就是构建器的任务。Amphitheatre 有三种类型的构建器 - **dockerfile**、**buildpacks** 和 **image**。

## Dockerfile

`dockerfile` 构建器是默认的构建器，在没有 `.amp.toml` 文件中的构建设置且存在 `Dockerfile` 时会被调用。它在当前目录中查找 `Dockerfile`，并使用它来构建可部署的镜像。如果您熟悉 Docker，您将对此选项感到熟悉。

这是最灵活的选项，但灵活性的代价是需要编写 `Dockerfile` 和 Docker 构建系统的相关特性。因此，Amphitheatre 还提供了更多简化此过程的构建选项。

## Buildpacks

像 Heroku 这样的平台使用了构建包（buildpack）的概念，这是一个完全在其自己的容器中运行的构建过程，用于构建可部署的镜像。然后，这些构建包被捆绑到一个具有操作系统的 "`builder`" 堆栈中，可以调用它们来构建应用程序。构建包的概念已经标准化，使用了 [Cloud Native Buildpacks](https://buildpacks.io/)。构建包使用多个测试来检测它们是否可以构建应用程序，如果可以，然后继续运行创建镜像所需的脚本。

[Paketo Buildpacks](https://paketo.io/) 提供了一组标准化的构建包库，其中包括 Heroku 的 Heroku20 构建包和 Amphitheatre 的构建包。如果要使用未列出的构建包，您可以使用 `.amp.toml` 中的 `buildpacks` 设置指定其名称。

构建包的配置选项 - 比如 [Heroku Nodejs 构建包](https://devcenter.heroku.com/articles/nodejs-support#using-npm-install) 中的 `YARN_PRODUCTION` - 可以通过 [Docker 构建参数](@/references/manifest.md) 进行设置。

使用构建包时，部署过程与其他构建器相同。

## Image

最后，如果您已经在仓库中拥有 Docker 镜像，并且只想部署它，您可以跳过构建过程，直接使用 `image` 构建选项进行部署。
