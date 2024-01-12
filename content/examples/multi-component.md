+++
title = "Multi-Component Applications"
description = "Learn how to deploy multi-component applications on Amphitheatre"
weight = 9
+++

## Introduction

A multi-component application refers to an application system composed of
multiple interrelated components or modules that work together. These components
may include different applications, services, middleware, and other relevant
modules that collaborate to achieve the complete functionality of the
application.

In software development, there are several common organizational approaches (see
below), and Amphitheatre supports these forms by specifying `partners`. These
forms can be combined, for example, in the same project, there may be
dependencies on both database middleware and another microservice interface.
These dependencies may be on a Git repository or may already be provided in an
official registry, as explained in the
[Partner Definition](@/references/manifest.md#the-partners-section) document.

### Monorepo

All related code, projects, and components are stored in a single version
control repository. For example, frontend and backend projects, different
applications, etc., are located in the same project directory for easy code
sharing and unified version management.

This can be supported by [Specifying path partners](@/references/manifest.md#specifying-path-partners) as shown below:

```toml
[partners]
hello_utils = { path = "hello_utils" }
```

### Multi-Repo

Different components, services, or modules are stored in their respective
independent version control repositories. In microservices architecture, each
microservice has its own repository, allowing independent development, testing,
and deployment, with communication through network protocols.

This can be supported by [Specifying partners from git repositories](@/references/manifest.md#specifying-partners-from-git-repositories) as shown below:

```toml
[partners]
bar = { repo = "https://github.com/foo/bar", branch = "next" }
```

### Middleware

Components have dependencies on each other, where one component may require
functionality or services provided by another component to accomplish specific
tasks. For example, the WordPress blog program depends on middleware software
such as MySQL and Redis to implement data storage and caching functionality.

This can be supported by [specifying partners from a registry](@/references/manifest.md#specifying-partners-from-registries) as shown below:

```toml
[partners]
# `mysql` partner from the `catalog` registry.
mysql = { version = "8.0", registry = "catalog" }

# `my-storage-service` partner from the `hub` registry.
my-storage-service = { version = "v1", registry = "hub" }
```

## The Example Application

**Bank of Anthos** is an HTTP-based web application example that simulates a
bank's payment processing network, allowing users to create artificial bank
accounts and complete transactions.

![Service Architecture](https://raw.githubusercontent.com/gsquared94/skaffold-remote-configs-demo/main/architecture.png)

| Service                                                     | Language   | Description                                                  |
| ----------------------------------------------------------- | ---------- | ------------------------------------------------------------ |
| `frontend` | Python     | Exposes an HTTP server to serve the website. Includes login page, registration page, and homepage. |
| `ledger-writer` | Java       | Accepts and validates incoming transactions before writing them to the ledger. |
| `balance-reader` | Java       | Provides an efficient read cache of user balances, such as from `ledger-db`. |
| `transaction-history` | Java       | Provides an efficient read cache of past transactions, such as from `ledger-db`. |
| `ledger-db` | PostgreSQL | Ledger for all transactions. Pre-populated with options for demonstration users. |
| `user-service` | Python     | Manages user accounts and authentication. Signs JWTs for authentication by other services. |
| `contacts` | Python     | Stores a list of other accounts associated with the user. Used for dropdown menus in "send payment" and "deposit" forms. |
| `accounts-db` | PostgreSQL | Database for user accounts and related data. Pre-populated with options for demonstration users. |
| `loadgenerator` | Python/Locust  | Constantly sends simulated user requests to the frontend. Periodically creates new accounts and simulates transactions between them. |

### Monorepo Version

The monorepo version has a simple manifest definition. Each service is defined
as a character, and their dependencies are specified in the main manifest using
the [Specified path partners](@/references/manifest.md#specifying-path-partners) method:

```toml
name = "frontend"

[partners]
userservice = { path = "userservice.amp.toml" }
contacts = { path = "contacts.amp.toml" }
ledgerwriter = { path = "ledgerwriter.amp.toml" }
balancereader = { path = "balancereader.amp.toml" }
transactionhistory = { path = "transactionhistory.amp.toml" }
```

The manifest definition for `userservice` is as follows. The definitions for the
other backend services are similar, and their details are not repeated here.

```toml
name = "userservice"

[partners]
postgres = { version = "16.1", registry = "catalog" }
```

It is important to note the `ledgerwriter` application, which, in addition to
depending on `postgres` like `userservice`, also depends on another
`balancereader` service:

```toml
name = "ledgerwriter"

[partners]
balancereader = { path = "balancereader.amp.toml" }
postgres = { version = "16.1", registry = "catalog" }
```

When building on the server side, Amphitheatre automatically resolves these
dependency relationships into a Directed Acyclic Graph (DAG), preventing issues
of redundant building or deployment of a certain dependency.

### Multi-Repo Version

This version modifies the original Bank of Anthos to a multi-repository
microservices design, demonstrating how to integrate with Amphitheatre's
"**specify partners from git repositories**" feature.

The manifest definition for the frontend application **`frontend`** is as
follows. It depends on several backend services such as `userservice`,
`contacts`, `ledgerwriter`, `balancereader`, `transactionhistory`, etc., all
introduced as dependencies through remote git repositories.

```toml
name = "frontend"

[partners]
userservice = { repo = "https://github.com/examples/bank-of-anthos-userservice" }
contacts = { repo = "https://github.com/examples/bank-of-anthos-contacts" }
ledgerwriter = { repo = "https://github.com/examples/bank-of-anthos-ledgerwriter" }
balancereader = { repo = "https://github.com/examples/bank-of-anthos-balancereader" }
transactionhistory = { repo = "https://github.com/examples/bank-of-anthos-transactionhistory" }
```

The manifest definitions for these backend services are placed in the root
directory of their respective repositories, following the standard convention of
naming the character manifest as `.amp.toml`. This allows the Amphitheatre
server to recognize this default name during parsing.

Additionally, unlike the "Monorepo" version, the manifest definition for
`ledgerwriter` needs to be changed from [Specifying path partners](@/references/manifest.md#specifying-path-partners) to
[Specifying partners from git repositories](@/references/manifest.md#specifying-partners-from-git-repositories).

```toml
name = "ledgerwriter"

[partners]
balancereader = { repo = "https://github.com/examples/bank-of-anthos-balancereader" }
postgres = { version = "16.1", registry = "catalog" }
```

## Deploying to Amphitheatre

For the monorepo version, execute the command in its project root directory. If
it's an example application of the multi-repo microservices version, please
navigate to the specific microservice's project directory (such as `frontend`):

```sh
amp run
```

This will search for the `.amp.toml` file in this project's root directory, and
`amp` will start deploying `frontend` and its dependencies to the Amphitheatre
platform.
