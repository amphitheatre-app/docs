+++
title = "index"
+++

# Amphitheatre Documentation

Amphitheatre is an open source developer platform that facilitates continuous development of applications and microservices. You can iterate your application source code locally, then deploy to a local or remote Kubernetes cluster, just like docker build && kubectl apply or docker-compose up. Amphitheatre handles the workflow of building, pushing, and deploying applications. It also provides building blocks and describes customization of CI/CD pipelines.

## Features

- Fast local Kubernetes Development

    - **Optimized “Source to Kubernetes”** - Amphitheatre detects changes in
      your source code and handles the pipeline to **build**, **push**, **test**
      and deploy your application automatically with **policy-based image
      tagging** and **highly optimized, fast local workflows**

    - **Continuous feedback** - Skaffold automatically manages deployment
      logging and resource port-forwarding

- Amphitheatre projects work everywhere

    - **Share with other developers** - Amphitheatre is the easiest way to
      **share your project** with the world: `git clone` and `amp run`

    - **Context aware** - use Amphitheatre profiles, local user config, environment
      variables, and flags to easily incorporate differences across environments

    - **CI/CD building blocks** - use `amp build`, `amp test` and `amp deploy`
      as part of your CI/CD pipeline, or simply `amp run` end-to-end

    - **GitOps integration** - use `amp render` to build your images and render
      templated Kubernetes manifests for use in GitOps workflows
    
- .amp.yml - a single pluggable, declarative configuration for your project

    - **amp init** - Amphitheatre can discover your build and deployment
      configuration and generate a Amphitheatre config
    
    - **Multi-component apps** - Amphitheatre supports applications with many
      components, making it great for microservice-based applications

    - **Bring your own tools** - Amphitheatre has a pluggable architecture, allowing
      for different implementations of the build and deploy stages
    
- Lightweight

    - **Client-side only** - Amphitheatre has no cluster-side component, so there’s
      no overhead or maintenance burden to your cluster
    
    - **Minimal pipeline** - Amphitheatre provides an opinionated, minimal pipeline
      to keep things simple

## Amphitheatre Workflow and Architecture 

Amphitheatre simplifies your development workflow by organizing common development stages into one simple command. Every time you run `amp dev`, the system

1. Collects and watches your source code for changes
2. Syncs files directly to pods if user marks them as syncable
3. Builds artifacts from the source code
4. Tests the built artifacts using
   [container-structure-tests](https://github.com/GoogleContainerTools/container-structure-test)
   or custom scripts
5. Tags the artifacts
6. Pushes the artifacts
7. Deploys the artifacts
8. Monitors the deployed artifacts
9. Cleans up deployed artifacts on exit (Ctrl+C)

> **Note**\
Any of these stages can be skipped.

The pluggable architecture is central to Amphitheatre's design, allowing you to use your preferred tool or technology in each stage. Also, Amphitheatre's profiles feature grants you the freedom to switch tools on the fly with a simple flag.

For example, if you are coding on a local machine, you can configure Amphitheatre to build artifacts with your local Docker daemon and deploy them to minikube using kubectl. When you finalize your design, you can switch to your production profile and start building with Google Cloud Build and deploy with Helm.

Amphitheatre supports the following tools:

#### IMAGE BUILDERS

- [Dockerfile](https://docs.docker.com/engine/reference/builder/)
    - locally with Docker
    - in-cluster with [Kaniko](https://github.com/GoogleContainerTools/kaniko)
    - on cloud with [Google Cloud Build](https://cloud.google.com/cloud-build/docs/)
- [Jib](https://github.com/GoogleContainerTools/jib) Maven and Gradle
    - locally
    - on cloud with Google Cloud Build
- [Bazel](https://bazel.build/) locally
- [Cloud Native Buildpacks](https://buildpacks.io/)
    - locally with Docker
    - on cloud with [Google Cloud Build](https://cloud.google.com/cloud-build/docs/)
- Custom script
    - locally
    - in-cluster

#### TESTERS

- [container-structure-test](https://github.com/GoogleContainerTools/container-structure-test)
- custom script

#### DEPLOYERS

- Kubernetes Command-Line Interface (kubectl)
- Helm
- kustomize


#### TAG POLICIES

- ag by git commit
- tag by current date & time
- tag by environment variables based template
- tag by digest of the Docker image


#### PUSH STRATEGIES

- don’t push - keep the image on the local daemon
- push to registry

![Architecture](/images/architecture.png)

Besides the above steps, Amphitheatre also automatically manages the following utilities for you:

- port-forwarding of deployed resources to your local machine using `kubectl
  port-forward`
- log aggregation from the deployed pods