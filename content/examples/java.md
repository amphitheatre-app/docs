+++
title = "Run a Java application"
description = "Learn how to deploy a Java application on Amphitheatre"
weight = 1
+++

Getting an application running on Amphitheatre is essentially working out how to
package it as a deployable image. Once packaged it can be deployed to the
Amphitheatre platform.

## The Example Application

Our example will be a basic "hello world" example using Java and [Spring
Boot](https://spring.io/projects/spring-boot).

You can get the code for the example from [the GitHub
repository](https://github.com/amphitheatre-app/amp-example-java). Just `git clone
https://github.com/amphitheatre-app/amp-example-java` to get a local copy. 

### Create a Simple Web Application

Now you can create a web controller for a simple web application, as the
following listing (from `src/main/java/hello/HelloController.java`) shows:

```java
package hello;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;

@RestController
public class HelloController {
    @RequestMapping("/")
    public String index() {
        return "Hello, World!";
    }
}
```

### Create an Application class

The Spring Initializr creates a simple application class for you. However, in
this case, it is too simple. You need to modify the application class to match
the following listing (from `src/main/java/hello/Application.java`):

```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## Run the Application

To run the application, run the following command in a terminal window (in the
`complete`) directory:

```
./mvnw spring-boot:run
```

Now run the service with curl (in a separate terminal window), by running the
following command (shown with its output):

```
$ curl localhost:8080
Hello, World!
```

## Install Amphitheatre

We are ready to start working with Amphitheatre and that means we need `amp`, our CLI
app for managing apps on Amphitheatre. If you've already installed it, carry on. If not,
hop over to [our installation guide](@/installation/_index.md). 

## Initialize the Character

To launch an app on Amphitheatre, run `amp init` in the directory with your source
code. This will create and configure a `Character` for you by inspecting your source
code, then prompt you to deploy.

```
$ amp init

Scanning source code
 Detected Java app
 Using the following build configuration
         Builder: paketobuildpacks/builder:base
		 Buildpacks: gcr.io/paketo-buildpacks/java
Wrote config file .amp.toml
Your Character is ready. run with `amp run`
...
```

First, this command scans your source code to determine how to build a
deployment image as well as identify any other configuration your app needs,
such as secrets and exposed ports.

After your source code is scanned and the results are printed, `amp` creates a
`Character` for you and writes your configuration to a `.amp.toml` file. You'll
then be prompted to build and deploy your character. Once complete, your app
will be running on Amphitheatre.

## Inside .amp.toml

The `.amp.toml` file now contains a default configuration for deploying your
`Character`. If we look at the `.amp.toml` file we can see it in there:

```toml
version = 1

[character]
name = "amp-example-java"
version = "0.0.1"
authors = ["Eguo Wang <wangeguo@gmail.com>"]
edition = "v1"
description = "A simple Java example app"
readme = "README.md"
homepage = "https://github.com/amphitheatre-app/amp-example-java"
repository = "https://github.com/amphitheatre-app/amp-example-java"
license = "Apache-2.0"
license-file = "LICENSE"
keywords = ["example", "java", "getting-started"]
categories = ["example"]
```

The amp command will always refer to this file in the current directory if it
exists, specifically for the Character name value at the start. That name will
be used to identify the Character to the Amphitheatre platform. The rest of the
file contains settings to be applied to the Character when it deploys.

See the [Paketo Java Buildpack
documentation](https://paketo.io/docs/howto/java/)
for more options.

## Deploying to Amphitheatre

To deploy your Character, just run:

```sh
amp run
```

This will lookup our `.amp.toml` file, and get the Character name `amp-example-java`
from there. Then `amp` will start the process of deploying our Character to the
Amphitheatre platform. `amp` will return you to the command line when it's done.

## Arrived at Destination

You have successfully built, deployed your first Java application on Amphitheatre.