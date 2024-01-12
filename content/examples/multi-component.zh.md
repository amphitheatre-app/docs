+++
title = "运行多组件应用"
description = "学习如何在 Amphitheatre 上部署多组件应用程序"
weight = 9
+++

## 多组件形态介绍

多组件应用程序是指由多个相互关联、协同工作的组件或模块组成的应用系统。这些组件可
以包括不同的应用程序、服务、中间件以及其他相关的模块，它们一起协同工作以实现应用
程序的完整功能。

在软件开发中，有几种常见的组织方式（见下），Amphitheatre 通过指定 `partners` 来
支持这些形式，且可组合使用，比如在同一个项目中，既有数据库中间件的依赖，也还依赖
另一个微服务接口，这些依赖可能是在Git 仓库上，也可能是在官方的注册中心已经提供
了，具体可参考 [Partner 定义](@/references/manifest.zh.md#partners-bu-fen)
文档说明。

### 单一仓库

所有相关的代码、项目以及组件都存储在一个单一的版本控制库（仓库）中。比如前端与
后端项目、不同应用程序等都位于同一个工程目录中，方便共享代码、统一版本管理。

我们可以通过[指定路径合作伙伴](@/references/manifest.zh.md#zhi-ding-lu-jing-he-zuo-huo-ban)
方式支持这种业界称为 Monorepo 项目的依赖关系，如下：

```toml
[partners]
hello_utils = { path = "hello_utils" }
```

### 多仓库

不同的组件、服务或模块分别存储在各自独立的版本控制 库中。如微服务架构中，每个微
服务有自己的仓库，独立开发、测试、部署，通过网络协议进行通信。

我们可以通过[指定来自 git 存储库的合作伙伴](@/references/manifest.zh.md#zhi-ding-lai-zi-git-cun-chu-ku-de-he-zuo-huo-ban)
方式支持这种项目的依赖关系，如下：

```toml
[partners]
bar = { repo = "https://github.com/foo/bar", branch = "next" }
```

### 中间件

组件之间存在依赖关系，其中一个组件可能需要另一个组件提供的功能或服务，以完成特定
的任务。如 WordPress 博客程序依赖于中间件软件如 MySQL 和 Redis，通过这些中间件实
现数据存储和缓存功能。

我们可以通过[指定来自注册表的合作伙伴](@/references/manifest.zh.md#zhi-ding-lai-zi-zhu-ce-biao-de-he-zuo-huo-ban)
方式支持这种依赖关系，如下：

```toml
[partners]
# 来自 `catalog` 注册表的 `mysql` 合作伙伴。
mysql = { version = "8.0", registry = "catalog" }

# 来自 `hub` 注册表的 `my-storage-service` 合作伙伴。
my-storage-service = { version = "v1", registry = "hub" }
```

## 示例程序

**Bank of Anthos** 是一个基于 HTTP 的 Web 应用程序示例，它模拟银行的支付处理网
络，允许用户创建人工银行账户并完成交易。

![Service Architecture](https://raw.githubusercontent.com/gsquared94/skaffold-remote-configs-demo/main/architecture.png)

| 服务                                                         | 语言       | 描述                                                         |
| ------------------------------------------------------------ | ---------- | ------------------------------------------------------------ |
| `frontend` | Python     | 公开 HTTP 服务器来为网站提供服务。包含登录页面、注册页面和主页。 |
| `ledger-writer` | Java       | 在将传入交易写入分类帐之前接受并验证它们。                   |
| `balance-reader` | Java       | 提供用户余额的高效可读缓存，如从`ledger-db`.                 |
| `transaction-history` | Java       | 提供过去事务的高效可读缓存，如从`ledger-db`.                 |
| `ledger-db` | PostgreSQL | 所有交易的分类账。为演示用户预先填充交易的选项。             |
| `user-service` | Python     | 管理用户帐户和身份验证。签署用于其他服务进行身份验证的 JWT。 |
| `contacts` | Python     | 存储与用户关联的其他帐户的列表。用于“发送付款”和“存款”表单中的下拉菜单。 |
| `accounts-db` | PostgreSQL | 用户帐户和相关数据的数据库。预填充演示用户的选项。           |
| `loadgenerator` | Python/Locust  | 不断向前端发送模拟用户的请求。定期创建新帐户并模拟它们之间的交易。 |



### 单一仓库版本

单一仓库的清单定义比较简单，将每个服务分别定义成一个 Character，然后在主清单中以
[指定路径合作伙伴](@/references/manifest.zh.md#zhi-ding-lu-jing-he-zuo-huo-ban)
的方式依赖它们：

```toml
name = "frontend"

[partners]
userservice = { path = "userservice.amp.toml" }
contacts = { path = "contacts.amp.toml" }
ledgerwriter = { path = "ledgerwriter.amp.toml" }
balancereader = { path = "balancereader.amp.toml" }
transactionhistory = { path = "transactionhistory.amp.toml" }
```

`userservice` 的清单定义如下，从架构图中可以得知，其它几个后端服务的定义是相似的，此处不再赘述。

```toml
name = "userservice"

[partners]
postgres = { version = "16.1", registry = "catalog" }
```

需特别说明 `ledgerwriter` 这个应用，它不仅如同 `userservice` 那样依赖
`postgres`，也还依赖了另个 `balancereader` 服务：

```toml
name = "ledgerwriter"

[partners]
balancereader = { path = "balancereader.amp.toml" }
postgres = { version = "16.1", registry = "catalog" }
```

Amphitheatre 在服务端构建时，会自动将以上依赖关系解析成 DAG（有向无环图），不会
发生重复构建或部署某个依赖荐的问题。

### 多仓库版本

该版本将原始 Bank of Anthos 修改为多仓库微服务设计，以演示如何与 Amphitheatre
"**指定来自 git 存储库的合作伙伴**" 功能集成。

前端应用程序 **`frontend`** 的清单定义如下，它依赖了 `userservice`, `contacts`,
`ledgerwriter`, `balancereader`, `transactionhistory` 等几个后端服务，均以远程
git 仓库的方式引入依赖

```toml
name = "frontend"

[partners]
userservice = { repo = "https://github.com/examples/bank-of-anthos-userservice" }
contacts = { repo = "https://github.com/examples/bank-of-anthos-contacts" }
ledgerwriter = { repo = "https://github.com/examples/bank-of-anthos-ledgerwriter" }
balancereader = { repo = "https://github.com/examples/bank-of-anthos-balancereader" }
transactionhistory = { repo = "https://github.com/examples/bank-of-anthos-transactionhistory" }
```

后端这几个服务的清单定义是放置在各自仓库的根目录下的，遵循常规 Character 清单标
准命名为 `.amp.toml`，这样 Amphitheatre 服务器在解析时才能识别到此默认名称。

另外，与 “单一仓库”版本不同的是，`ledgerwriter` 的清单定义要从
[指定路径合作伙伴](@/references/manifest.zh.md#zhi-ding-lu-jing-he-zuo-huo-ban)换成
[指定来自 git 存储库的合作伙伴](@/references/manifest.zh.md#zhi-ding-lai-zi-git-cun-chu-ku-de-he-zuo-huo-ban)。

```toml
name = "ledgerwriter"

[partners]
balancereader = { repo = "https://github.com/examples/bank-of-anthos-balancereader" }
postgres = { version = "16.1", registry = "catalog" }
```

## 部署到 Amphitheatre

单一仓库版本就是在它的工程根目录下执行命令，如果是多仓库微服务版本的示例应用，请
到具体微服务的工程目录（如 `frontend`）：

```sh
amp run
```

这将查找此工程根目录下的 `.amp.toml` 文件，然后 `amp` 将开始部署 `frontend` 及其依赖的后端到 Amphitheatre 平台。
