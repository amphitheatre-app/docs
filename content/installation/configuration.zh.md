+++
title = "配置 Amphitheatre"
description = "配置本地环境或远程 Kubernetes 集群"
weight = 2
+++

安装 Amphitheatre 后，您需要初始化一些配置，其中一个更重要的配置是凭据的配置。创
建一个 `Secret`，用于 Docker Registry 的推送凭据，您计划使用
[`Builder`](@/concepts/builders.zh.md) 发布 OCI 镜像。您的配置应该如下所示：

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

您可以从 [GitHub仓库](https://github.com/amphitheatre-app/k8s-manifests-example)
获取示例的清单, 包括一些关于 K8s 的配置和一些示例清单。
