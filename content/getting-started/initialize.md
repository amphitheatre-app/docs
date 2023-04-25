+++
title = "Initialize Amphitheatre in your infrastructure"
description = "Configure your local environment or remote Kubernetes cluster"
weight = 2

[extra.sidebar]
title = "Init Amphitheatre locally"
+++

## Configure the cluster

We use [kpack](https://github.com/pivotal/kpack) to perform the build of the code to the image, kpack extends Kubernetes and utilizes unprivileged kubernetes primitives to provide builds of OCI images as a platform implementation of [Cloud Native Buildpacks](https://buildpacks.io/) (CNB).

kpack provides a declarative builder resource that configures a Cloud Native Buildpacks build configuration with the desired buildpack order and operating system stack.

So, after installing Amphitheatre, you need to initialize some configurations, one of the more important ones being the configuration of kpack custom resources.

### 1. Create a credentials configuration

Create a ConfigMap with push credentials for the docker registry that you plan on publishing OCI images to with kpack. Your configuration create should look something like this:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: amp-configurations
  namespace: amp-system
data:
  confgiuration.toml: |
    [registry."https://index.docker.io/v1/"]
    username = '<username>'
    password = '<password>'

    [repositories]
```

Apply that configuration to the cluster

```bash
kubectl apply -n amp-system -f amp-configurations.yaml
```

### 2. Create a cluster store configuration

A store resource is a repository of [buildpacks](http://buildpacks.io/) packaged in [buildpackages](https://buildpacks.io/docs/buildpack-author-guide/package-a-buildpack/) that can be used by kpack to build OCI images. Later in this tutorial you will reference this store in a Cluster Builder configuration.

We recommend starting with buildpacks from the [paketo project](https://github.com/paketo-buildpacks). The example below pulls in java and nodejs buildpacks from the paketo project.

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

Apply this store to the cluster

```bash
kubectl apply -f cluster-store.yaml
```

> Note: Buildpacks are packaged and distributed as buildpackages which are docker images available on a docker registry. Buildpackages for other languages are available from [paketo](https://github.com/paketo-buildpacks).

### 3. Create a cluster stack configuration

A stack resource is the specification for a [cloud native buildpacks stack](https://buildpacks.io/docs/concepts/components/stack/) used during build and in the resulting app image.

We recommend starting with the [paketo base stack](https://github.com/paketo-buildpacks/stacks) as shown below:

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

Apply this stack to the cluster

```bash
kubectl apply -f cluster-stack.yaml
```

### 4. Create a Cluster Builder configuration

A Cluster Builder is the kpack configuration for a builder image that includes the stack and buildpacks needed to build an OCI image from your app source code.

The Cluster Builder configuration will write to the registry with the secret configured in step one and will reference the stack and store created in step three and four. The builder order will determine the order in which buildpacks are used in the builder.

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

> Note.
 1. `<Namespace>` is the domain name and space of Docker Registry, please fill in the real value with the Registries information in the first place.
 2. `serviceAccountRef` is the [`controllers.serviceAccount.name`](https://github.com/amphitheatre-app/charts/blob/master/charts/amphitheatre/) you filled in when you installed the server software values.yaml#L180), please change this value according to the real situation.

Apply this builder to the cluster

```bash
kubectl apply -f cluster-builder.yaml
```

The execution time of the above resources depends on the internet speed at the time of pulling the image, you may have to wait a few minutes or so for all resources to be applied successfully and then you can access Amphitheatre.

> Note: Learn more buildpacks and kpack with the [kpack documentation](https://github.com/pivotal/kpack)
