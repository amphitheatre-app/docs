+++
title = "Configure the Amphitheatre"
description = "Configure your local environment or remote Kubernetes cluster"
weight = 2
+++

After installing Amphitheatre, you need to initialize some configurations, one of the more important ones being the configuration of credentials.

Create a `Secret` with push credentials for the Docker Registry that you plan on publishing OCI images to with [`Builder`](@/concepts/builders.md). Your configuration create should look something like this:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: amp-credentials
  namespace: amp-system
stringData:
  credentials: |
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

Apply that configuration to the cluster

```bash
kubectl apply -n amp-system -f amp-credentials.yaml
```

You can get the manifests for the example from the [GitHub repository](https://github.com/amphitheatre-app/k8s-manifests-example), Include some configuration about k8s, and some example manifests.
