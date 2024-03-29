+++
title = "测试"
description = "测试贡献者指南"
weight = 50
+++

### 测试您的代码

Amphitheathre 使用 GitHub 操作来在合并之前对任何PR运行自动化测试。然而，在所有测
试都通过之前，PR 不会被审查，因此为了节省时间并防止您的 PR 过时，最好在提交 PR
之前进行测试。

### 开启拉取请求

### 草稿模式

您可以在[草稿模式](https://github.blog/2019-02-14-introducing-draft-pull-requests)
下开启拉取请求。所有自动化测试仍将运行在PR上，但 PR 不会被指定为待审查。一旦 PR 准备好进行审查，
请将其从草稿模式切换，并通知代码所有者。

### PR合并前提条件

为了使 PR 能够合并，应满足以下条件：
1. PR 已通过所有自动化测试（样式、构建和符合性测试）。
2. PR 提交已使用 `--signoff` 选项签名。
3. PR 已由代码所有者审查并批准。
4. PR 已重新基于上游的主分支。
