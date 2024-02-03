+++
title = "创建 Buildpack"
description = "通过 Packer 便捷地构建 Buildpack 项目，使得您能够更专注于业务逻辑的实现。"
weight = 2
sort_by = "weight"

[extra.sidebar]
linkable = true
+++

本文将以 `Leo` 智能合约为例，阐述如何开发一个 `Buildpack`。

`Buildpack` 构成了整个架构的基础层，其代码分散在多个仓库之中。

以 `leo-dist buildpack` 和 `aleo buildpack` 为例，这两个包分别实现了业务逻辑的关键功能，这些功能是大多数区块链项目都需要具备的。

例如，`leo-dist buildpack` 负责合约的编译工作；而 `aleo buildpack` 则负责钱包的初始化、devnet 网测试币获取、合约的编译与部署等关键操作。

这些操作通常需要较长的执行时间，这也反映出对智能合约和区块链技术的深入理解和熟练掌握。

## 整体流程

以下是初始化 `Buildpack` 的步骤：

1. 创建一个名为 `config.toml` 的 `Buildpack` 配置文件。

2. 在 `config.toml` 文件中，指定您希望组合的 `Buildpack` 依赖项，例如：

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

3. 利用 `Packer CLI` 工具来初始化您的 `Buildpack` 项目。

4. 运行 `scripts/build.sh` 脚本，以生成 `Buildpack` 所需的 `bin` 命令。

5. 运行官方 `pack build` 命令来构建您的 `Buildpack`。

## 初始化项目

### 1. 直接创建项目及对应文件夹

```bash
packer init -c config.toml leo-dist
```

> 底层原始包通常以 `-dist` 作为其命名后缀，比如官方的 `rust-dist`。

### 2. 在空文件夹中使用

首先，进入一个空的文件夹，然后运行：

```bash
cd leo-dist
packer init -c config.toml
```

### 3. 强制覆盖已存在的项目

如果您希望覆盖一个已经存在的项目，可以使用以下命令：

```bash
packer init -f -c config.toml leo-dist
```

> 请注意，使用 `-f` 选项将强制覆盖现有的项目及其中所有文件内容。在执行此操作之前，请确保您已经对项目进行了完整备份，以防不测。

> 至此，`leo-dist Buildpack` 项目已成功创建，然而，接下来需要进一步编写和完善业务逻辑。

查看项目文件树，您将会发现以下相关文件已自动创建。

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

## 编写业务逻辑

### 1. 完善 `leo/detect.go` 逻辑

以下是用于生成模板的关键部分。请注意，TODO 部分需要根据您的具体需求进行修改或补充。

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

### 2. 完善 `leo/build.go` 逻辑

同样，以下是用于生成模板的关键部分。请注意，TODO 部分需要根据您的具体需求进行修改或补充。

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

### 3. 完善 `leo/leo.go` 逻辑

同样，以下是模板生成代码的关键部分。请留意，TODO 部分需根据您的具体需求进行相应的修改或补充。

在 `leo buildpack` 包中，`Contribute` 函数承载着构建包的核心逻辑。您需要依据您的项目需求完善此函数，例如设置环境变量、编译工程等。

而对于 `aleo buildpack` 包，您需要依据具体需求来实现相关功能，这可能包括部署环境的初始化、devnet 网的测试币获取、智能合约的部署等操作。

请根据您的项目需求和构建流程，对上述提到的函数进行适当的定制和扩展。

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

> 以下是 AMP 官方提供的两个 Buildpack 实现仓库，请予以查阅：

* leo: https://github.com/amp-buildpacks/leo-dist/tree/main/leo
* aleo: https://github.com/amp-buildpacks/aleo/tree/main/aleo

> 这些链接将引导您进入相应的 GitHub 仓库，其中包含了 AMP 官方支持的 Buildpack 实现细节和文档。
