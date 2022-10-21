+++
title = "Introduction"
description = "Welcome to the Amphitheatre documentation site!"
+++

## What is Amphitheatre?

Amphitheatre is an open source developer platform that facilitates continuous
development of applications and microservices. You can iterate your application
source code locally, then deploy to a local or remote Kubernetes cluster, just
like docker build && kubectl apply or docker-compose up. Amphitheatre handles
the workflow of building, pushing, and deploying applications. It also provides
building blocks and describes customization of CI/CD pipelines.

## Features

- Fast local Kubernetes Development

    - **Optimized “Source to Kubernetes”** - Amphitheatre detects changes in
      your source code and handles the pipeline to **build**, **push**, **test**
      and deploy your application automatically with **policy-based image
      tagging** and **highly optimized, fast local workflows**

    - **Continuous feedback** - Amphitheatre automatically manages deployment
      logging and resource port-forwarding

- Amphitheatre projects work everywhere

    - **Share with other developers** - Amphitheatre is the easiest way to
      **share your project** with the world: `git clone` and `amp run`

    - **Context aware** - use Amphitheatre profiles, local user config,
      environment variables, and flags to easily incorporate differences across
      environments

    - **CI/CD building blocks** - use `amp build`, `amp test` and `amp deploy`
      as part of your CI/CD pipeline, or simply `amp run` end-to-end

    - **GitOps integration** - use `amp render` to build your images and render
      templated Kubernetes manifests for use in GitOps workflows
    
- .amp.toml - a single pluggable, declarative configuration for your project

    - **amp init** - Amphitheatre can discover your build and deployment
      configuration and generate a Amphitheatre config
    
    - **Multi-component apps** - Amphitheatre supports applications with many
      components, making it great for microservice-based applications

    - **Bring your own tools** - Amphitheatre has a pluggable architecture,
      allowing for different implementations of the build and deploy stages
    
- Lightweight
    
    - **Minimal pipeline** - Amphitheatre provides an opinionated, minimal
      pipeline to keep things simple

## Workflow 

Amphitheatre simplifies your development workflow by organizing common
development stages into one simple command. Every time you run `amp dev`, the
system

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

Amphitheatre also automatically manages the following utilities for you:

- port-forwarding of deployed resources to your local machine using `kubectl
  port-forward`
- log aggregation from the deployed pods