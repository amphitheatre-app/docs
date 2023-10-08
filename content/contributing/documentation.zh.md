+++
title = "文档"
description = "帮助改进 Amphitheatre 文档。"
weight = 20
+++

我们很高兴看到您对为 Amphitheatre 文档做出贡献感兴趣。如果这是您首次为开源项目做出贡献，我们强烈建议阅读 GitHub 的优秀指南：如何[为开源项目做出贡献](https://opensource.guide/how-to-contribute)。

## 寻找需要处理的文档问题

您可以在此处找到一份有关文档相关的开放问题列表：[问题列表](https://github.com/amphitheatre-app/docs/issues)。当您找到想要处理的问题时：

- 通过评论表达您有兴趣开始处理问题。
- 其中一个维护者会为您分配该问题。
- 您可以开始处理该问题。完成后，只需提交拉取请求。

## 文档拉取请求的要求

当您创建一个新的拉取请求时，我们希望满足一些要求。

- 请遵循以下拉取请求的命名约定：
- 当添加新文档时，在标题之前添加 `New Documentation:`，例如: `New Documentation: Getting Started`
- 当修复文档时，在标题之前添加 `Fix Documentation:`，例如：`Fix Documentation: Getting Started`
- 当更新文档时，在标题之前添加 `Update Documentation:`，例如：`Update Documentation: Getting Started`
- 如果您的拉取请求关闭了一个问题，您必须写上 `Closes #ISSUE_NUMBER`，其中问题编号是链接末尾的数字或相关问题的编号。这将链接您的拉取请求与问题，当它被合并时，问题将关闭。
- 对于每个拉取请求，我们运行测试以检查是否存在任何损坏的链接、Markdown 格式是否有效以及是否通过了代码检查。

## 本地测试文档站点

运行 Zola 的本地实例以开发 Amphitheatre 文档。

```bash
zola serve
```
启用自动更新的 Zola 本地构建和服务，然后访问 http://127.0.0.1:1111。
