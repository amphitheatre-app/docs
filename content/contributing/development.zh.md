+++
title = "开发"
description = "帮助改进 Amphitheatre。"
weight = 10
+++

感谢您抽出时间为 Amphitheatre 做出贡献。以下是有关为 Amphitheatre 做出贡献的指南和说明。

在为我们的代码库做出贡献之前，请首先考虑通过开启一个问题来讨论您希望进行的更改。

### 推荐阅读

- [Rust](https://www.rust-lang.org/learn/get-started)（CLI、后端 / Api 服务器）
- [Flutter](https://docs.flutter.dev/)（桌面版）
- [Zola](https://www.getzola.org/documentation/getting-started/overview/)（文档）

###  如何提交拉取请求？

如果你的贡献需要对项目存储库进行更改，请查看 `DEVELOPMENT.md` 文件（如果存储库有
的话），以确保你已安装了先决条件。它可能还包括其他必要的步骤（如运行测试的说
明），但总体上，你将需要执行以下步骤：

- [分叉（Fork）](https://docs.github.com/zh/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo) 存储库。
- [克隆（Clone）](https://docs.github.com/zh/repositories/creating-and-managing-repositories/cloning-a-repository) 你的分叉存储库。
- 为问题创建一个分支：`git checkout -b {{BRANCH_NAME}}`
- 进行任何必要的更改。
- 使用签名提交你的更改：`git commit -s`

签名是添加到你的提交消息的单行，证明你编写和/或拥有贡献的权利。签名应该如下所示：

```
Signed-off-by: John Doe <john.doe@email.com>
```

另外，通过在提交命令中添加 -s 标志，git 可以自动添加签名，如 `git commit -s`。

认证的完整文本在[此处](https://developercertificate.org/)可用。

- 推送到 GitHub：`git push origin {{BRANCH_NAME}}`
- [创建拉取请求](https://docs.github.com/zh/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork)。

### 代码测试

所有提交的PR都会经过一系列的测试和审查。您可以在提交PR之前运行大多数这些测试。事
实上，我们建议这样做，因为它将节省许多可能的审查迭代和自动化测试。测试指南可以在
这里找到：

- [贡献者测试指南](@/contributing/testing.zh.md)

### 许可证

通过贡献，您同意您的贡献将按照相关存储库的许可证文件中所述进行许可。
