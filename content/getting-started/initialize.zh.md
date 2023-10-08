+++
title = "在您的基础设施中初始化 Amphitheatre"
description = "配置本地环境或远程 Kubernetes 集群"
weight = 2

[extra.sidebar]
title = "在本地初始化 Amphitheatre"
+++

## 配置集群

我们使用 [kpack](https://github.com/pivotal/kpack) 来执行代码构建为镜像，kpack 扩展了 Kubernetes 并利用非特权的 Kubernetes 原语来提供 OCI 镜像的构建，作为 [Cloud Native Buildpacks](https://buildpacks.io/) (CNB) 的平台实现。

kpack 提供了一个声明性的构建器资源，用于配置所需的构建包和操作系统堆栈的 Cloud Native Buildpacks 构建配置。

因此，在安装 Amphitheatre 之后，您需要初始化一些配置，其中一个更重要的配置之一是 kpack 自定义资源的配置。您可以从 [GitHub仓库](https://github.com/amphitheatre-app/k8s-manifests-example) 获取示例的清单。包括一些关于 K8s 的配置和一些示例清单。

### 1. 创建凭证配置

创建一个 ConfigMap，其中包含发布 OCI 镜像时将要使用的 Docker Registry 的推送凭据。您的配置应该类似于这样：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: amp-configurations
  namespace: amp-system
data:
  configuration.toml: |
    [[registries]]
    name = "Docker Hub"
    default = true
    server = "https://index.docker.io/v1/"
    username = "<username>"
    password = "<password>"
    token = "<token>"

    [[repositories]]
    name = "GitHub"
    driver = "github"
    server = "https://github.com"
    username = "<username>"
    password = "<password>"
    token = "<token>"
```

将该配置应用到集群

```bash
kubectl apply -n amp-system -f amp-configurations.yaml
```

### 2. 创建一个集群存储配置

存储资源是 [buildpacks](http://buildpacks.io/) 打包在 [buildpackages](https://buildpacks.io/docs/buildpack-author-guide/package-a-buildpack/) 中的存储库，可以由 kpack 用于构建 OCI 镜像。在本教程的后面部分，您将在 Cluster Builder 配置中引用此存储。

我们建议从 [paketo项目](https://github.com/paketo-buildpacks) 开始使用构建包。下面的示例从 paketo 项目中拉取了 Java 和 Nodejs 构建包。

```yaml
apiVersion: kpack.io/v1alpha2
kind: ClusterStore
metadata:
  name: amp-default-cluster-store
spec:
  sources:
  - image: gcr.io/paketo-buildpacks/go
  - image: gcr.io/paketo-buildpacks/java
  - image: gcr.io/paketo-buildpacks/nodejs
  - image: gcr.io/paketo-buildpacks/php
  - image: gcr.io/paketo-buildpacks/python
  - image: gcr.io/paketo-buildpacks/ruby
  - image: docker.io/paketocommunity/rust
  - image: gcr.io/paketo-buildpacks/dotnet-core
```

将此存储应用到集群

```bash
kubectl apply -f cluster-store.yaml
```

> 注意：构建包被打包和分发为 buildpackages，它们是 docker 镜像，可以在 docker registry 上使用。其他语言的 buildpackages 可以从 [paketo](https://github.com/paketo-buildpacks) 获取。

### 3. 创建一个集群堆栈配置

堆栈资源是在构建期和生成的应用程序镜像中使用的 [cloud native buildpacks stack](https://buildpacks.io/docs/concepts/components/stack/) 的规范。

我们建议从 [paketo基础堆栈](https://github.com/paketo-buildpacks/stacks) 开始，如下所示：

```yaml
apiVersion: kpack.io/v1alpha2
kind: ClusterStack
metadata:
  name: amp-default-cluster-stack
spec:
  id: "io.buildpacks.stacks.bionic"
  buildImage:
    image: "paketobuildpacks/build:full-cnb"
  runImage:
    image: "paketobuildpacks/run:full-cnb"
```

将此堆栈应用到集群

```bash
kubectl apply -f cluster-stack.yaml
```

### 4. 创建一个 Cluster Builder 配置

Cluster Builder 是用于从应用程序源代码构建OCI镜像所需的堆栈和构建包的 kpack 配置。

Cluster Builder 配置将使用在步骤一中配置的密钥向注册表写入，并将引用在步骤三和四中创建的堆栈和存储。构建器的顺序将确定构建器中构建包的使用顺序。

```yaml
apiVersion: kpack.io/v1alpha2
kind: ClusterBuilder
metadata:
  name: amp-default-cluster-builder
spec:
  tag: <namespace>/amp-default-cluster-builder
  stack:
    name: amp-default-cluster-stack
    kind: ClusterStack
  store:
    name: amp-default-cluster-store
    kind: ClusterStore
  serviceAccountRef:
    name: amp-controllers
    namespace: amp-system
  order:
  - group:
    - id: paketo-buildpacks/go
  - group:
    - id: paketo-buildpacks/java
  - group:
    - id: paketo-buildpacks/nodejs
  - group:
    - id: paketo-buildpacks/php
  - group:
    - id: paketo-buildpacks/python
  - group:
    - id: paketo-buildpacks/ruby
  - group:
    - id: paketo-community/rust
  - group:
    - id: paketo-buildpacks/dotnet-core
```

1. `<Namespace>` 是 Docker Registry 的域名和空间，请根据第一步中 Registries 信息的实际值填写。

2. `serviceAccountRef` 是在安装服务器软件时填写的 [`controllers.serviceAccount.name`](https://github.com/amphitheatre-app/charts/blob/master/charts/amphitheatre/values.yaml#L180)，请根据实际情况更改此值。

将此构建器应用到集群

```bash
kubectl apply -f cluster-builder.yaml
```

上述资源的执行时间取决于拉取镜像时的互联网速度，您可能需要等待几分钟或更长时间，然后才能访问 Amphitheatre。

> 注意：了解更多有关 buildpacks 和 kpack的信息，请查阅 [kpack文档](https://github.com/pivotal/kpack)
