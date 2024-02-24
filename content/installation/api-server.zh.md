+++
title = "安装 API 服务器"
description = "在 Kubernetes 集群上使用 Helm 安装 Amphitheatre API 服务器。"
weight = 1
+++

您可以通过 Helm 在 Kubernetes 上部署 Amphitheatre API 服务器，使其具有高可用性。
通过这种方式，如果承载 Amphitheatre API 服务器的节点之一不可用，用户将不会受到服
务中断的影响。

## 先决条件

- Kubernetes 1.19+
- Helm 3.9.0+
- 底层基础设施支持 PV 配置器
- 用于部署扩展的 **`ReadWriteMany`** 卷

## 添加仓库

在正确设置 Helm 后，按以下步骤添加仓库：

```sh
  helm repo add amphitheatre https://charts.amphitheatre.app
```

## 安装

要使用发布名称 `amp` 安装图表：

```sh
  helm install amp amphitheatre/amphitheatre --create-namespace --namespace=amp-system
```

该命令会在 Kubernetes 集群中部署 Amphitheatre API 服务器，使用默认配置。

### 指标

在 Kubernetes 中，Metrics Server 是一个用于监控容器资源使用情况的关键组件，它是
Amphitheatre 项目中 Stats 功能的先决依赖项。在 Helm Chart
中，`metrics-server.enabled` 参数默认为 `true`，确保在 Amphitheatre 安装期间自动
安装 Metrics Server。

但是，如果您的集群中已经运行了全局 Metrics Server 并且希望避免多余的安装，您可以
通过在安装 Amphitheatre 时设置 `--set metrics-server.enabled=false` 参数来禁用
Metrics Server 的安装。

### 持久化存储

在具有多个节点的群集中部署时，由 `amp-controllers` 创建的 PVC要求 `access_modes`
为 `["ReadWriteMany"]`，因此您需要将 `persistence.storageClass` 设置为支持该模式
的 `StorageClass`，例如 `--set persistence.storageClass=standard` 和 `--set
persistence.accessMode=ReadWriteMany`。

> 请正确设置 `storageClass` 和 `accessMode`，否则将无法正常工作。

如果你的 Kubernetes 集群没有提供合适的 StorageClass，你可以尝试使用 [Dynamic NFS Volume Provisioner](https://github.com/openebs/dynamic-nfs-provisioner)。

## 升级

如果您之前已经添加过该仓库，请运行 `helm repo update` 来获取软件包的最新版本。然
后您可以运行 `helm search repo amphitheatre` 来查看图表。

升级原 `amp` 图表：

```sh
  helm upgrade amp amphitheatre/amphitheatre --create-namespace --namespace=amp-system
```

## 卸载

```sh
  helm delete amp
```

该命令会删除与该图表关联的所有 Kubernetes 组件，并删除该发布。

## 接下来

成功安装后，您仍然需要进行一些初始配置工作，详情请参阅在[您的基础架构中初始化
Amphitheatre](@/installation/configuration.zh.md)。

## 故障排除

如果在安装 Amphitheatre API 服务器时遇到任何问题，请查看故障排除 FAQ 中的错误消息。

有关更多信息，请访问 [Amphitheatre 官方 Helm 图表仓库](https://github.com/amphitheatre-app/charts)。
