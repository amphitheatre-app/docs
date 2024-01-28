+++
title = "Aptos"
description = "如何在 Aptos 上开发智能合约程序"
weight = 1
+++

在 Amphitheatre 上运行 Web2 应用程序与 Web3 应用程序有所差异，因此，部署 Aptos 智能合约之前，您需要先构建合约，然后，将合约部署到集群内的 DevNet 上，以此查看合约的运行情况。

## 示例应用程序

您可以从 [GitHub 存储库](https://github.com/amphitheatre-app/amp-example-aptos) 获
取示例的代码。只需运行以下命令以获取本地副本：`git clone
https://github.com/amphitheatre-app/amp-example-aptos`。

`amp-example-aptos` 应用程序是一个小型示例，正如您所期望的那样。它是一个 Aptos 智能合约。

`sources/todolist.move` 代码如下所示：

```rust
module todolist_addr::todolist {

    use aptos_framework::account;
    use std::signer;
    use aptos_framework::event;
    use std::string::String;
    use aptos_std::table::{Self, Table};
    #[test_only]
    use std::string;

    // Errors
    const E_NOT_INITIALIZED: u64 = 1;
    const ETASK_DOESNT_EXIST: u64 = 2;
    const ETASK_IS_COMPLETED: u64 = 3;

    struct TodoList has key {
        tasks: Table<u64, Task>,
        task_counter: u64
    }

    #[event]
    struct Task has store, drop, copy {
        task_id: u64,
        address: address,
        content: String,
        completed: bool,
    }

    public entry fun create_list(account: &signer) {
        let todo_list = TodoList {
            tasks: table::new(),
            task_counter: 0
        };
        // move the TodoList resource under the signer account
        move_to(account, todo_list);
    }

    public entry fun create_task(account: &signer, content: String) acquires TodoList {
        // gets the signer address
        let signer_address = signer::address_of(account);
        // assert signer has created a list
        assert!(exists<TodoList>(signer_address), E_NOT_INITIALIZED);
        // gets the TodoList resource
        let todo_list = borrow_global_mut<TodoList>(signer_address);
        // increment task counter
        let counter = todo_list.task_counter + 1;
        // creates a new Task
        let new_task = Task {
            task_id: counter,
            address: signer_address,
            content,
            completed: false
        };
        // adds the new task into the tasks table
        table::upsert(&mut todo_list.tasks, counter, new_task);
        // sets the task counter to be the incremented counter
        todo_list.task_counter = counter;
        // fires a new task created event
        event::emit(new_task);
    }

    public entry fun complete_task(account: &signer, task_id: u64) acquires TodoList {
        // gets the signer address
        let signer_address = signer::address_of(account);
        // assert signer has created a list
        assert!(exists<TodoList>(signer_address), E_NOT_INITIALIZED);
        // gets the TodoList resource
        let todo_list = borrow_global_mut<TodoList>(signer_address);
        // assert task exists
        assert!(table::contains(&todo_list.tasks, task_id), ETASK_DOESNT_EXIST);
        // gets the task matched the task_id
        let task_record = table::borrow_mut(&mut todo_list.tasks, task_id);
        // assert task is not completed
        assert!(task_record.completed == false, ETASK_IS_COMPLETED);
        // update task as completed
        task_record.completed = true;
    }

    #[test(admin = @0x123)]
    public entry fun test_flow(admin: signer) acquires TodoList {
        // creates an admin @todolist_addr account for test
        account::create_account_for_test(signer::address_of(&admin));
        // initialize contract with admin account
        create_list(&admin);

        // creates a task by the admin account
        create_task(&admin, string::utf8(b"New Task"));
        let todo_list = borrow_global<TodoList>(signer::address_of(&admin));
        assert!(todo_list.task_counter == 1, 5);
        let task_record = table::borrow(&todo_list.tasks, todo_list.task_counter);
        assert!(task_record.task_id == 1, 6);
        assert!(task_record.completed == false, 7);
        assert!(task_record.content == string::utf8(b"New Task"), 8);
        assert!(task_record.address == signer::address_of(&admin), 9);

        // updates task as completed
        complete_task(&admin, 1);
        let todo_list = borrow_global<TodoList>(signer::address_of(&admin));
        let task_record = table::borrow(&todo_list.tasks, 1);
        assert!(task_record.task_id == 1, 10);
        assert!(task_record.completed == true, 11);
        assert!(task_record.content == string::utf8(b"New Task"), 12);
        assert!(task_record.address == signer::address_of(&admin), 13);
    }

    #[test(admin = @0x123)]
    #[expected_failure(abort_code = E_NOT_INITIALIZED)]
    public entry fun account_can_not_update_task(admin: signer) acquires TodoList {
        // creates an admin @todolist_addr account for test
        account::create_account_for_test(signer::address_of(&admin));
        // account can not toggle task as no list was created
        complete_task(&admin, 2);
    }
}
```

## 构建应用程序

与大多数 Aptos 应用程序一样，简单的 `aptos move compile` 将创建一个可运行的二进制文件。因此，
原始应用程序可以正常运行。现在，将其打包以供 Amphitheatre 使用。

## 安装 Amphitheatre

我们已准备好开始使用 Amphitheatre，并且这意味着我们需要 `amp`，这是用于管理
Amphitheatre 上应用程序的 CLI 应用程序。如果您已经安装了它，请继续。如果没有，请
跳转到 [我们的安装指南](@/installation/_index.zh.md)。

## 在 .amp.toml 中

`.amp.toml` 文件现在包含用于部署您的 `Character` 的默认配置。如果我们查看
`.amp.toml` 文件，可以在其中看到：

```toml
name = "amp-example-aptos"
version = "0.1.0"
edition = "v1"
description = "A simple Aptos example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-aptos"
repository = "https://github.com/amphitheatre-app/amp-example-aptos"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "aptos", "getting-started"]
categories = ["example"]

[build]
builder = "ghcr.io/amp-buildpacks/move-builder"

[partners]
aptos = { version = "1.9.2", registry = "catalog" }

[build.env]
BP_ENABLE_APTOS_DEPLOY = "false"
# BP_APTOS_DEPLOY_PRIVATE_KEY = "<Your-Deploy-Private-Key>"
```

如果该文件存在，amp 命令将始终引用当前目录中的此文件，特别是开始时的 Character
名称值。该名称将用于标识 Character 到 Amphitheatre 平台。文件的其余部分包含部署
Character 时要应用的设置。

有关更多选项，请参阅 [move-builder 文档](https://github.com/amp-buildpacks/move-builder)。

## 部署到 Amphitheatre

要部署您的 Character，请运行：

```sh
amp run
```

这将查找我们的 `.amp.toml` 文件，并从中获取 Character 名称 `amp-example-aptos`。然
后，`amp` 将开始部署 Character 到 Amphitheatre 平台。`amp` 将在完成时返回到命令
