+++
title = "Introduction"
description = "Welcome to the Amphitheatre documentation site!"
+++

> **IMPORTANT!!!**\
This document is still in progress, content will be updated at any time, some
links or guides are previews and do not represent real availability, please do
not use them for official production environments.

## What is Amphitheatre?

**Amphitheatre** is an open-source developer platform designed to assist developers in quickly launching new automated development environments in the cloud. It provides on-demand, pre-configured tools, libraries, and dependencies, ensuring you can start coding immediately. You can edit your application source code locally, and Amphitheatre will automatically incrementally deploy your changes to a Kubernetes cluster, making the transition between local development and remote deployment smoother and more efficient.

## Features

- **No dependencies** No need to configure the local development environment and support for multiple programming languages and frameworks.

- **Unlimited isolation environments** Rapidly generate countless isolated integration test environments without any limitations, streamlining the test process and reducing costs
- **Multi-component applications** Supports applications with multiple components, ideal for microservices-based applications and collaborative multi-person development.
- **Live Update** Lets you skip image builds altogether and update running containers with new code in seconds instead of minutes.
- **Integrations** Integrate development, testing, deployment, and other tools to create a comprehensive software integration solution, automating workflow processes to streamline software development.
- **Snapshots** Snapshots lets you share your dev environment and collaborate on issues as quickly as looking at the monitor next to you.

## Start developing with Amphitheatre

{{ grid(columns=3)}}
{{ column() }}{{ card(title="Getting started", text="How to get up and running with Amphitheatre in your environment in minutes.", url="/getting-started/") }}{{ end() }}
{{ column() }}{{ card(title="Core Concepts", text="Learn about Amphitheatre, including its main features and capabilities.", url="/concepts/") }} {{ end() }}
{{ column() }}{{ card(title="Installation Guide", text="Learn how to install and configure the Amphitheatre in your infrastructure.", url="/installation/") }} {{ end() }}
{{ end() }}

## Language & Framework Guides

{{ grid(columns=3)}}
{{ column() }}{{ card(title="Golang", text="Learn how to deploy a Go application.", url="/examples/golang/") }}{{ end() }}
{{ column() }}{{ card(title="Python", text="Learn how to deploy a Python application.", url="/examples/python/") }}{{ end() }}
{{ column() }}{{ card(title="Java", text="Learn how to deploy a Java application.", url="/examples/java/") }}{{ end() }}
{{ column() }}{{ card(title="NodeJs", text="Learn how to deploy a NodeJs application.", url="/examples/nodejs") }}{{ end() }}
{{ column() }}{{ card(title="Rust", text="Learn how to deploy a Rust application.", url="/examples/rust") }}{{ end() }}
{{ column() }}{{ card(title="PHP", text="Learn how to deploy a PHP application.", url="/examples/php") }}{{ end() }}
{{ end() }}

## Additional info

{{ grid(columns=2)}}
{{ column() }}{{ card(title="References", text="Detailed documentation on the Amphitheatre API, CLI and more.", url="/references/") }}{{ end() }}
{{ column() }}{{ card(title="Contributing", text="How to contribute to the Amphitheatre project and the various repositories.", url="/contributing/") }} {{ end() }}
{{ end() }}
