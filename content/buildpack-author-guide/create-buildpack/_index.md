+++
title = "Create a Buildpack"
description = "Quickly build a Buildpack project using Packer, allowing you to focus more on the implementation of business logic."
weight = 2
sort_by = "weight"

[extra.sidebar]
linkable = true
+++

This document will use the `Leo` smart contract as an example to explain how to develop a `Buildpack`.

The `Buildpack` forms the foundational layer of the entire architecture, with its code scattered across multiple repositories.

Taking `leo-dist buildpack` and `aleo buildpack` as examples, these two packages implement key functionalities of the business logic, which are required by most blockchain projects.

For instance, `leo-dist buildpack` is responsible for the compilation of contracts, while `aleo buildpack` handles the initialization of wallets, obtaining test coins on the devnet, and the deployment of contracts.

These operations usually require a significant amount of execution time, reflecting a deep understanding and proficiency in smart contract and blockchain technology.

## Overall Process

Here are the steps to initialize a `Buildpack`:

1. Create a `config.toml` file for the `Buildpack`.

2. In the `config.toml` file, specify the `Buildpack` dependencies you wish to combine, for example:

```toml
[[dependencies]]
id = "leo-gnu"
name = "Leo (GNU libc)"
pkg_name = "leo"
sha256 = "abcd29454e940dd320b6915569f840a9ffe2515045c06667b5aa2ad34f7e0320"
uri = "https://github.com/AleoHQ/leo/releases/download/v1.10.0/leo-v1.10.0-x86_64-unknown-linux-gnu.zip"
version = "1.10.0"
license = "GNU"

[[dependencies]]
id = "leo-musl"
name = "Leo (musl libc)"
pkg_name = "leo"
sha256 = "508264f03760d0a0c9d8cd13c603e0e0d595388b3729762ebfbcc26abe46d667"
uri = "https://github.com/AleoHQ/leo/releases/download/v1.10.0/leo-v1.10.0-x86_64-unknown-linux-musl.zip"
version = "1.10.0"
license = "GNU"
```

3. Use the `Packer CLI` tool to initialize your `Buildpack` project.

4. Execute the `scripts/build.sh` script to generate the `bin` commands required for the `Buildpack`.

5. Run the official `pack build` command to build your `Buildpack`.

## Initializing the Project

### 1. Create the project and corresponding folders directly

```bash
packer init -c config.toml leo-dist
```

> The underlying raw packages usually have `-dist` as their suffix, such as the official `rust-dist`.

### 2. Use in an empty folder

First, navigate to an empty folder, then run:

```bash
cd leo-dist
packer init -c config.toml
```

### 3. Forcefully overwrite an existing project

If you wish to overwrite an existing project, you can use the following command:

```bash
packer init -f -c config.toml leo-dist
```

> Please note that using the `-f` option will forcefully overwrite all contents of the existing project. Make sure to back up your project completely before performing this operation to avoid any unforeseen issues.

> At this point, the `leo-dist Buildpack` project has been successfully created, but you will need to further write and perfect the business logic.
Checking the project file tree, you will find the following related files have been automatically created:

```bash
> tree .github .
.github
├── pipeline-descriptor.yml
├── workflows
│   └── pb-update-pipeline.yml
├── LICENSE
├── README.md
├── buildpack.toml
├── cmd
│   └── main
│       └── main.go
├── go.mod
├── go.sum
├── leo
│   ├── build.go
│   ├── detect.go
│   └── leo.go
└── scripts
    └── build.sh
```

## Write Business Logic

### 1. Refine the logic in `leo/detect.go`

Below is the key part for generating templates. Please note that the `TODO` section needs to be modified or supplemented according to your specific requirements.

```go
func (d Detect) leoProject(appDir string) (bool, error) {
	// TODO: update project config filename, like package.json or Move.toml
	filename := "<filename>"
	_, err := os.Stat(filepath.Join(appDir, filename))
	if os.IsNotExist(err) || err != nil {
		return false, fmt.Errorf("unable to determine if %s exists\n%w", filename, err)
	}

	buildDirectory := filepath.Join(appDir, ".")
	// TODO: update extension
	extension := ".xxx"
	if err := existsFilesWithExtension(buildDirectory, extension); err != nil {
		return false, fmt.Errorf("unable to determine if '%s' exists\n%w", extension, err)
	}
	return true, nil
}
```

### 2. Refine the logic in `leo/build.go`

Similarly, here is the key part for generating templates. Please note that the `TODO` section needs to be modified or supplemented according to your specific requirements.

```go
// TODO: update dependency.id from metadata.dependencies for buildpack.toml
v, _ := cr.Resolve("BP_LEO_VERSION")
dependency, err := dr.Resolve("leo", v)
if err != nil {
    return libcnb.BuildResult{}, fmt.Errorf("unable to find dependency\n%w", err)
}

leoLayer := NewLeo(dependency, dc)
leoLayer.Logger = b.Logger
```

### 3. Refine the Logic in `leo/leo.go`

Once again, here is the critical part of the code for generating templates. Please note that the TODO section requires modifications or additions based on your specific needs.

Within the `leo buildpack` package, the `Contribute` function hosts the core logic of the buildpack. You will need to flesh out this function according to your project's requirements, such as setting environment variables, compiling the project, etc.

For the `aleo buildpack` package, you will need to implement related features based on specific requirements, which may include initializing the deployment environment, obtaining test tokens on the devnet, deploying smart contracts, and more.

Please customize and extend the aforementioned functions appropriately based on your project's needs and build process.

```go
func (r Leo) Contribute(layer libcnb.Layer) (libcnb.Layer, error) {
	r.LayerContributor.Logger = r.Logger
	return r.LayerContributor.Contribute(layer, func(artifact *os.File) (libcnb.Layer, error) {
		// TODO: update below codes to your buildpack
		bin := filepath.Join(layer.Path, "bin")

		// TODO: May be use copy instead of it or update Extract Path or stripComponents=1
		r.Logger.Bodyf("Expanding %s to %s", artifact.Name(), bin)
		if err := crush.Extract(artifact, bin, 1); err != nil {
			return libcnb.Layer{}, fmt.Errorf("unable to expand %s\n%w", artifact.Name(), err)
		}

		// Must be set to executable
		// TODO: May be update bin-name
		file := filepath.Join(bin, "leo")
		r.Logger.Bodyf("Setting %s as executable", file)
		if err := os.Chmod(file, 0755); err != nil {
			return libcnb.Layer{}, fmt.Errorf("unable to chmod %s\n%w", file, err)
		}

		// Must be set to PATH
		r.Logger.Bodyf("Setting %s in PATH", bin)
		if err := os.Setenv("PATH", sherpa.AppendToEnvVar("PATH", ":", bin)); err != nil {
			return libcnb.Layer{}, fmt.Errorf("unable to set $PATH\n%w", err)
		}

		// get leo version
		buf, err := r.Execute("leo", []string{"--version"})
		if err != nil {
			return libcnb.Layer{}, fmt.Errorf("unable to get leo version\n%w", err)
		}
		version := strings.TrimSpace(buf.String())
		r.Logger.Bodyf("Checking leo version: %s", version)

		// TODO: May be need more codes...
		return layer, nil
	})
}

func (r Leo) BuildProcessTypes(cr libpak.ConfigurationResolver, app libcnb.Application) ([]libcnb.Process, error) {
	processes := []libcnb.Process{}

	enableDeploy := cr.ResolveBool("BP_ENABLE_LEO_PROCESS")
	if enableDeploy {
		// TODO: Update run command and args
		processes = append(processes, libcnb.Process{
			Type:    "web",
			Command: "<run-command>",
			Arguments: []string{"<command-args>"},
			Default: true,
		})
	}
	return processes, nil
}
```

> Below are two GitHub repositories provided by AMP for the implementation of Buildpacks, please refer to them:

* [leo-dist buildpack](https://github.com/amp-buildpacks/leo-dist/tree/main/leo)
* [aleo buildpack](https://github.com/amp-buildpacks/aleo/tree/main/aleo)

> These links will direct you to the corresponding GitHub repositories, which contain the details and documentation of the Buildpack implementations supported by AMP officially.
