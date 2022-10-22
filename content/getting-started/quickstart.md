+++
title = "Quickstart"
description = "Hit the ground running with our quickstarts, complete with code samples aimed to get you started quickly with Amphitheatre."
weight = 3
+++

Learn how to start using Amphitheatre on an example project that is hosted on GitHub in less than 5 minutes. For simplicity we use GitHub as the git hoster but the steps outlined work equally well for GitLab and Bitbucket. This section helps you understand the features and advantages of Amphitheatre in a learning environment. 

> **Note**\
We are actively working on adding to our quickstart library. In the meantime, you can explore Amphitheatre through our tutorials.

## Before you begin
- [Set up your local Amphitheatre environment](@/getting-started/installation.md).

All templates are pre-configured to use Amphitheatre and ready-to-code:

## Templates

{{ grid(columns=2) }}
{{ column() }}{{ card(title="More on Golang", url="/getting-started/quickstart/go", color="text-bg-light") }}{{ end() }}
{{ column() }}{{ card(title="More on Java Spring", url="/getting-started/quickstart/java") }}{{ end() }}
{{ column() }}{{ card(title="More on NodeJs - Typescript", url="/getting-started/quickstart/nodejs") }}{{ end() }}
{{ column() }}{{ card(title="More on Python Django", url="/getting-started/quickstart/python") }}{{ end() }}
{{ column() }}{{ card(title="More on Rust", url="/getting-started/quickstart/rust") }}{{ end() }}
{{ column() }}{{ card(title="More on Svelte", url="/getting-started/quickstart/svelte") }}{{ end() }}
{{ end() }}

## Prerequisites

Jekyll requires the following:

- Ruby version 2.5.0 or higher
- RubyGems
- GCC and Make

See Requirements for guides and details.

## Instructions

1. Install all prerequisites.
2. Install the jekyll and bundler gems.
    ```
    gem install jekyll bundler
    ```
3. Create a new Jekyll site at ./myblog.
    ```
    jekyll new myblog
    ```
4. Change into your new directory.
    ```
    cd myblog
    ```
5. Build the site and make it available on a local server.
    ```
    bundle exec jekyll serve
    ```
6. Browse to http://localhost:4000

If you encounter any errors during this process, check that you have installed all the prerequisites in Requirements. If you still have issues, see Troubleshooting.

Installation varies based on your operating system. See our guides for OS-specific instructions.

## Tell us what you think!

We’re continuously working to improve our Quickstart examples and value your feedback. Did you find this Quickstart helpful? Do you have suggestions for improvement?

Join the discussion in our discord channel.