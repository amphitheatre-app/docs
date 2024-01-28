+++
title = "Aptos"
description = "How to develop smart contract programs on Aptos"
weight = 1
+++

Running Web2 applications on Amphitheatre differs from running Web3 applications, therefore, before deploying Aptos smart contracts, you need to first build the contracts, then deploy them to the DevNet within the cluster to observe the execution of the contracts.

## The Example Application

You can get the code for the example from [the GitHub
repository](https://github.com/amphitheatre-app/amp-example-aptos). Just `git clone
https://github.com/amphitheatre-app/amp-example-aptos` to get a local copy.

The `amp-example-aptos` application is, as you'd expect for an example, small. It's a Aptos
smart contract. 

Here's all the code from `sources/todolist.move`:

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

## Building the Application

As with most Aptos applications, a simple `aptos move compile` will create a binary
which we can run. So, the raw application works. Now to package
it up for Amphitheatre.

## Install Amphitheatre

We are ready to start working with Amphitheatre and that means we need `amp`, our CLI
app for managing apps on Amphitheatre. If you've already installed it, carry on. If not,
hop over to [our installation guide](@/installation/_index.md).

## Inside .amp.toml

The `.amp.toml` file now contains a default configuration for deploying your
`Character`. If we look at the `.amp.toml` file we can see it in there:

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

The amp command will always refer to this file in the current directory if it
exists, specifically for the Character name value at the start. That name will
be used to identify the Character to the Amphitheatre platform. The rest of the
file contains settings to be applied to the Character when it deploys.

See the [move-builder
documentation](https://github.com/amp-buildpacks/move-builder)
for more options.

## Deploying to Amphitheatre

To deploy your Character, just run:

```sh
amp run
```

This will lookup our `.amp.toml` file, and get the Character name `amp-example-aptos`
from there. Then `amp` will start the process of deploying our Character to the
Amphitheatre platform. `amp` will return you to the command line when it's done.

## Arrived at Destination

You have successfully built, deployed your first Aptos application on Amphitheatre.
