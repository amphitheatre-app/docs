+++
title = "Installing API Server"
description = "Installing the Amphitheatre API Server with Helm on a Kubernetes cluster."
weight = 1
+++

You can deploy Amphitheatre API Server on Kubernetes via helm to make it highly
available. In this way, if one of the nodes on which Amphitheatre API Server is
running becomes unavailable, users do not experience interruptions of service.

## Pre-requisites

- Kubernetes 1.19+
- Helm 3.2.0+
- PV provisioner support in the underlying infrastructure
- ReadWriteMany volumes for deployment scaling

## Add Repository

Once Helm has been set up correctly, add the repo as follows:

```sh
  helm repo add amphitheatre https://charts.amphitheatre.app
```

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo
amphitheatre` to see the charts.

## Installation

To install the chart with the release name `amp`:

```sh
  helm install amp amphitheatre/amphitheatre --create-namespace --namespace=amp-system
```

The command deploys Amphitheatre API Server on the Kubernetes cluster in the
default configuration.

## Uninstall

```sh
  helm delete amp
```

The command removes all the Kubernetes components associated with the chart and
deletes the release.

## Next

When successfully installed, you still need to do some initial configuration work,
the details of which can be found in the [Initialize Amphitheatre in your infrastructure](@/getting-started/initialize.md).

## Troubleshooting

If you have any trouble installing Amphitheatre API Server, look for the error
message in the Troubleshooting FAQ.

See the [Amphitheatre official Helm charts
repository](https://github.com/amphitheatre-app/charts) for more information.
