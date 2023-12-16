+++
title = "清单格式"
weight = 1
+++

每个角色的 `.amp.toml` 文件称为其 *清单*。它采用 [TOML] 格式编写。每个清单文件都包含以下部分：

* [`meta`](#meta-bu-fen) — 定义角色。
  * [`name`](#name-zi-duan) — 角色的名称。
  * [`version`](#version-zi-duan) — 角色的版本。
  * [`authors`](#authors-zi-duan) — 角色的作者。
  * [`description`](#description-zi-duan) — 角色的描述。
  * [`documentation`](#documentation-zi-duan) — 角色文档的 URL。
  * [`readme`](#readme-zi-duan) — 角色 README 文件的路径。
  * [`homepage`](#homepage-zi-duan) — 角色主页的 URL。
  * [`repository`](#repository-zi-duan) — 角色源代码仓库的 URL。
  * [`license`](#license-he-license-file-zi-duan) — 角色许可证。
  * [`license-file`](#license-he-license-file-zi-duan) — 许可证文本的路径。
  * [`keywords`](#keywords-zi-duan) — 角色的关键词。
  * [`categories`](#categories-zi-duan) — 角色所属的类别。
  * [`exclude`](#exclude-he-include-zi-duan) — 发布时要排除的文件。
  * [`include`](#exclude-he-include-zi-duan) — 发布时要包含的文件。
  * [`publish`](#publish-zi-duan) — 可用于阻止发布角色。
* [`[partners]`](#partners-bu-fen) — 合作伙伴依赖项。

### meta 部分

`.amp.toml` 中的第一个部分是元数据。

```toml
name = "hello_world" # 角色的名称
version = "0.1.0"    # 当前版本，遵循语义化版本规范
authors = ["Alice <a@example.com>", "Bob <b@example.com>"]
```

Amphitheatre 需要的唯一字段是 [`name`](#name-zi-duan) 和
[`version`](#version-zi-duan)。如果要发布到注册表，注册表可能需要额外的字段。请
查看下面的说明以及 [发布章节][publishing]，了解发布到 [Registry] 所需的要求。

#### `name` 字段

角色名称是用于引用角色的标识符。当在其他角色中列出为合作伙伴时，以及作为隐含目标
的默认名称时使用。

名称必须仅包含字母数字字符或 `-` 或 `_`，且不能为空。请注意，[`amp init`] 对角色
名称施加了一些额外的限制，例如强制它是有效的 Rust 标识符而不是关键字。[Registry]
强制执行更多的限制，例如仅支持 ASCII 字符、不是保留名称、不是特殊的 Windows 名称
（如 "nul"）、不太长等等。

#### `version` 字段

Amphitheatre 包含了 [语义化版本规范](https://semver.org/) 的概念，因此请确保遵循
一些基本规则：
* 使用具有三个数字部分（例如 1.0.0）的版本号，而不是 1.0。

#### `authors` 字段

可选的 `authors` 字段列出被视为角色的 "作者" 的人员或组织。确切的含义可以根据需
要解释 — 可以列出原始或主要作者、当前维护者或角色的所有者。每个作者条目的末尾可
以包含一个可选的电子邮件地址，地址要放在尖括号中。

> **警告**：已发布版本的角色清单无法更改，因此在已发布的角色版本中无法更改或删除此字段。

#### `description` 字段

`description` 是有关角色的简短描述。[Registry] 将显示此描述。此字段应为纯文本
（不是 Markdown）。

```toml
# ...
description = "我的角色的简短描述"
```

> **注意**：[Registry] 要求设置 `description` 字段。

#### `documentation` 字段

`documentation` 字段指定角色文档托管的网站的 URL。

```toml
# ...
documentation = "https://docs.rs/bitflags"
```

#### `readme` 字段

`readme` 字段应该是角色根目录中的文件路径（相对于这个 `.amp.toml`），该文件包含
有关角色的一般信息。当您发布时，此文件将传输到注册表。[Registry] 将将其解释为
Markdown 并在角色页面上呈现它。

```toml
# ...
readme = "README.md"
```

如果没有为此字段指定值，并且角色根目录中存在名为 `README.md`、`README.txt` 或
`README` 的文件，则将使用该文件的名称。您可以通过将此字段设置为 `false` 来抑制此
行为。如果将字段设置为 `true`，则会假定默认值为 `README.md`。

#### `homepage` 字段

`homepage` 字段应该是角色主页的 URL。

```toml
# ...
homepage = "https://example.com/"
```

#### `repository` 字段

`repository` 字段应该是角色源代码仓库的 URL。

```toml
# ...
repository = "https://github.com/amphitheatre-app/amp-example-go/"
```

#### `license` 和 `license-file` 字段

`license` 字段包含角色所发布的软件许可证的名称。`license-file` 字段包含包含许可
证文本的文件的路径（相对于这个 `.amp.toml`）。

[Registry] 将 `license` 字段解释为 [SPDX 2.3 许可证表达
式][spdx-2.3-license-expressions]。名称必须是 [SPDX 许可证列表
3.20][spdx-license-list-3.20] 中的已知许可证之一。当前不支持括号。有关更多信息，
请参阅[SPDX 网站]。

SPDX 许可证表达式支持 AND 和 OR 运算符，以组合多个许可证。

```toml
# ...
license = "MIT OR Apache-2.0"
```

使用 `OR` 表示用户可以选择任一许可证。使用 `AND` 表示用户必须同时遵守两个许可
证。`WITH` 运算符表示带有特殊例外的许可证。以下是一些示例：

* `MIT OR Apache-2.0`
* `LGPL-2.1-only AND MIT AND BSD-2-Clause`
* `GPL-2.0-or-later WITH Bison-exception-2.2`

如果角色使用非标准许可证，则可以在 `license` 字段之外指定 `license-file` 字段。

```toml
# ...
license-file = "LICENSE.txt"
```

> **注意**：[Registry] 要求 `license` 或 `license-file` 中的至少一个字段被设置。

#### `keywords` 字段

`keywords` 字段是一个描述此角色的字符串数组。这有助于在注册表上搜索角色时，并且
您可以选择任何有助于他人找到此角色的单词。

```toml
# ...
keywords = ["gamedev", "graphics"]
```

> **注意**：[Registry] 最多支持 5 个关键字。每个关键字必须是 ASCII 文本，以字母
> 开头，只包含字母、数字、`_` 或 `-`，并且最多包含 20 个字符。

#### `categories` 字段

`categories` 字段是一个字符串数组，表示此角色所属的类别。

```toml
categories = ["command-line-utilities", "development-tools::build-plugins"]
```

> **注意**：[Registry] 最多支持 5 个类别。每个类别应与
> <https://registry.amphitheatre.app/category_slugs> 中可用的字符串之一匹配，并且必须精确匹配。

#### `exclude` 和 `include` 字段

`exclude` 和 `include` 字段可用于明确指定在打包角色以进行[发布][publishing]时包
含哪些文件以及某些类型的更改跟踪（下面描述）。在 `exclude` 字段中指定的模式标识
一组不包括的文件，而 `include` 中的模式指定了明确包含的文件。

```toml
# ...
exclude = ["/ci", "images/", ".*"]
```

```toml
# ...
include = ["/src", "COPYRIGHT", "/examples", "!/examples/big_example"]
```

如果未指定这两个字段中的任何一个，则默认情况下将包括来自角色根目录的所有文件，但
排除下面列出的文件。

如果未指定 `include`，则将排除以下文件：

* 如果角色不在 git 存储库中，将跳过以点开头的所有 "隐藏" 文件。
* 如果角色在 git 存储库中，则将跳过存储库和全局 git 配置的 [gitignore] 规则所忽略的任何文件。

无论是指定了 `exclude` 还是 `include`，始终会排除以下文件：

* 任何子角色都将被跳过（任何包含 `.amp.toml` 文件的子目录）。
* 在角色根目录中命名为 `target` 的目录将被跳过。

以下文件始终包括在内：

* 角色自身的 `.amp.toml` 文件始终包括在内，无需在 `include` 中列出。
* 如果角色包含二进制或示例目标，则自动包括最小化的 `.amp.playbook`。


选项是互斥的；设置 `include` 将覆盖 `exclude`。如果需要对一组 `include` 文件进行
排除，请使用下面描述的 `!` 运算符。

这些模式应为 [gitignore] 样式的模式。简要来说：

- `foo` 匹配角色中任何位置的名称为 `foo` 的文件或目录。这等同于模式 `**/foo`。
- `/foo` 仅匹配角色根目录中名称为 `foo` 的文件或目录。
- `foo/` 匹配角色中任何位置的名称为 `foo` 的*目录*。
- 支持常见的通配符模式，如 `*`、`?` 和 `[]`：
  - `*` 匹配除 `/` 以外的零个或多个字符。例如，`*.html` 匹配角色中任何位置的扩展名为 `.html` 的文件或目录。
  - `?` 匹配除 `/` 以外的任何字符。例如，`foo?` 匹配 `food`，但不匹配 `foo`。
  - `[]` 允许匹配一系列字符。例如，`[ab]` 匹配 `a` 或 `b`。`[a-z]` 匹配字母 a 到 z。
- `**/` 前缀在任何目录中匹配。例如，`**/foo/bar` 匹配角色中任何位置直接在 `foo` 目录下的文件或目录中的 `bar`。
- `/**` 后缀匹配内部的所有内容。例如，`foo/**` 匹配 `foo` 目录中的所有文件，包括 `foo` 下的所有子目录中的文件。
- `/**/` 匹配零个或多个目录。例如，`a/**/b` 匹配 `a/b`、`a/x/b`、`a/x/y/b` 等。

`!` 前缀否定了一个模式。例如，`src/*.rs` 和 `!foo.rs` 的模式将匹配 `src` 目录中
所有具有 `.rs` 扩展名的文件，但不匹配名为 `foo.rs` 的任何文件。

[gitignore]: https://git-scm.com/docs/gitignore

#### `publish` 字段

`publish` 字段可用于防止意外发布到角色注册表（例如 *[Registry]*）中的角色。例
如，为了在公司中保持私有角色。

```toml
# ...
publish = false
```

值也可以是允许发布到的注册表名称的字符串数组。

```toml
# ...
publish = ["some-registry-name"]
```

如果 publish 数组包含一个单一注册表，当未指定 `--registry` 标志时，`amp publish`
命令将使用它。

### `[partners]` 部分

您的角色可以依赖于 [Registry] 或其他注册表、git 存储库或项目的其他角色。您可以针
对不同的平台具有不同的合作伙伴，并且在开发过程中只使用的合作伙伴。让我们看看如何
执行这些操作。

#### 指定来自注册表的合作伙伴

在注册表中共享常见的软件组件可以简化开发，节省时间和精力。它增加了代码重用，减少
了复杂性，并确保可靠的资源管理。通过集中的问题解决，增强了开发速度，减小了错误，
通过中央化问题解决提高了软件稳定性。

```toml
[partners]
### 来自 `catalog` 注册表的 `mysql` 合作伙伴。
mysql = { version = "8.0", registry = "catalog" }

### 来自 `hub` 注册表的 `my-storage-service` 合作伙伴。
my-storage-service = { version = "v1", registry = "hub" }
```

#### 指定来自 git 存储库的合作伙伴

要依赖于位于 `git` 存储库中的角色，您需要指定存储库的位置，并且最低信息是使用
`repo` 键指定存储库的位置：

```toml
[partners]
bar = { repo = "https://github.com/foo/bar" }
```

Amphitheatre 将获取此位置的 `git` 存储库，然后在存储库的任何位置（不一定在根目录
—— 例如，项目的 `path`）查找所请求角色的 `.amp.toml`。

由于我们没有指定任何其他信息，Amphitheatre 假定我们打算使用默认分支上的最新提交
来构建角色，该分支不一定是主分支。您可以将 `repo` 键与 `rev`、`tag` 或 `branch`
键相结合，以指定其他内容。以下是指定要使用名为 `next` 的分支上的最新提交的示例：

```toml
[partners]
bar = { repo = "https://github.com/foo/bar", branch = "next" }
```

不是分支或标签的任何内容都属于 `rev`。这可以是像 `rev = "4c59b707"` 这样的提交哈
希，也可以是远程存储库公开的命名引用，例如 `rev = "refs/pull/493/head"`。可用的
引用因存储库托管的位置而异；GitHub 特别公开了每个拉取请求的最新提交的引用，如上
所示，但其他 git 主机通常以不同的命名方式提供类似的内容。

#### 指定路径合作伙伴

Amphitheatre 支持**路径合作伙伴**，这些合作伙伴通常是位于同一存储库中的子角色。

```toml
[partners]
hello_utils = { path = "hello_utils" }
```

这告诉 Amphitheatre 我们依赖于名为 `hello_utils` 的角色，该角色位于
`hello_utils` 文件夹中（相对于根目录）。

就是这样！下一次运行 `amp build` 时，将自动构建 `hello_utils` 及其所有合作伙伴，
其他人也可以开始使用该角色。

### 示例 `.amp.toml` 文件

这个示例 `amp.toml` 展示了如何在单个文件中组合多个设置。它不是所有可用配置选项的
全面示例。

```toml
name = "amp-example-go"
version = "0.0.2"
authors = ["Eguo Wang <wangeguo@gmail.com>"]
edition = "v1"
description = "A simple Golang example app"
documentation = "https://docs.amphitheatre.app/examples/golang/"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-go"
repository = "https://github.com/amphitheatre-app/amp-example-go"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "golang", "getting-started"]
categories = ["example"]

[partners]
mysql = { version = "8.0", registry = "catalog" }
my-storage-service = { version = "v1", registry = "hub" }
bar = { repo = "https://github.com/foo/bar", branch = "master" }
another-local-service = { path = "pkg/another-local-service" }
```

上述示例中的清单文件包含了很多字段，但您可以根据需要包含或排除不同字段。在实际使
用中，您可能不会使用所有这些字段。

希望这些信息有助于您编写正确格式的 .amp.toml 文件。如果您有更多问题，请随时提
出。

[`amp init`]: @/cli/init.md
[`amp run`]: @/cli/run.md
[registry]: https://github.com/amphitheatre-app/catalog
[publishing]: publishing
[spdx-2.3-license-expressions]: https://spdx.github.io/spdx-spec/v2.3/SPDX-license-expressions/
[spdx-license-list-3.20]: https://github.com/spdx/license-list-data/tree/v3.20
[SPDX 网站]: https://spdx.org
[TOML]: https://toml.io/
