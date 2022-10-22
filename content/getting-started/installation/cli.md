+++
title = "Installing Amphitheatre CLI"
description = "Install the Amphitheatre Command Line Tool, It's optional."
weight = 3

[extra]
toc = false
+++

For detailed install instructions, follow the guide for your operating system.

{{ tabs(id="os", items=["Linux", "macOS", "Windows", "Docker"])}}

{{ pane(tab="os", index=1) }}

The latest stable binaries can be found here:

- Linux x86_64 (amd64): [https://repo.amphitheatre.app/cli/releases/latest/amp-linux-amd64](https://repo.amphitheatre.app/cli/releases/latest/amp-linux-amd64)
- Linux ARMv8 (arm64): [https://repo.amphitheatre.app/cli/releases/latest/amp-linux-arm64](https://repo.amphitheatre.app/cli/releases/latest/amp-linux-arm64)

Simply download the appropriate binary and add it to your PATH. Or, copy+paste
one of the following commands in your terminal:

```
# For Linux x86_64 (amd64)
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-linux-amd64 && \
sudo install amp /usr/local/bin/
```

```
# For Linux ARMv8 (arm64)
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-linux-arm64 && \
sudo install amp /usr/local/bin/
```

We also release a bleeding edge build, built from the latest commit:

- Linux x86_64 (amd64): [https://repo.amphitheatre.app/cli/builds/latest/amp-linux-amd64](https://repo.amphitheatre.app/cli/builds/latest/amp-linux-amd64)
- Linux ARMv8 (arm64): [https://repo.amphitheatre.app/cli/builds/latest/amp-linux-arm64](https://repo.amphitheatre.app/cli/builds/latest/amp-linux-arm64)

```
# For Linux on x86_64 (amd64)
curl -Lo amp https://repo.amphitheatre.app/cli/builds/latest/amp-linux-amd64 && \
sudo install amp /usr/local/bin/
```

```
# For Linux on ARMv8 (arm64)
curl -Lo amp https://repo.amphitheatre.app/cli/builds/latest/amp-linux-arm64 && \
sudo install amp /usr/local/bin/
```

{{ end() }}

{{ pane(tab="os", index=2) }}

The latest stable binaries can be found here:

- Darwin x86_64 (amd64): [https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-amd64](https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-amd64)
- Darwin ARMv8 (arm64): [https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-arm64](https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-arm64)

Simply download the appropriate binary and add it to your PATH. Or, copy+paste
one of the following commands in your terminal:

```
# For macOS on x86_64 (amd64)
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-amd64 && \
sudo install amp /usr/local/bin/
```

```
# For macOS on ARMv8 (arm64)
curl -Lo amp https://repo.amphitheatre.app/cli/releases/latest/amp-darwin-arm64 && \
sudo install amp /usr/local/bin/
```

We also release a bleeding edge build, built from the latest commit:

- Darwin x86_64 (amd64): [https://repo.amphitheatre.app/cli/builds/latest/amp-darwin-amd64](https://repo.amphitheatre.app/cli/builds/latest/amp-darwin-amd64)
- Darwin ARMv8 (arm64): [https://repo.amphitheatre.app/cli/builds/latest/amp-darwin-arm64](https://repo.amphitheatre.app/cli/builds/latest/amp-darwin-arm64)

```
# For macOS on x86_64 (amd64)
curl -Lo amp https://repo.amphitheatre.app/cli/builds/latest/amp-darwin-amd64 && \
sudo install amp /usr/local/bin/
```

```
# For macOS on ARMv8 (arm64)
curl -Lo amp https://repo.amphitheatre.app/cli/builds/latest/amp-darwin-arm64 && \
sudo install amp /usr/local/bin/
```

Amphitheatre CLI is also kept up to date on a few central package managers:

### Homebrew

```
brew install amp
```

### MacPorts

```
sudo port install amp
```

{{ end() }}

{{ pane(tab="os", index=3) }}

The latest stable release binary can be found here:

[https://repo.amphitheatre.app/cli/releases/latest/amp-windows-amd64.exe](https://repo.amphitheatre.app/cli/releases/latest/amp-windows-amd64.exe)

Simply download it and place it in your `PATH` as `amp.exe`.

We also release a **bleeding edge** build, built from the latest commit:

[https://repo.amphitheatre.app/cli/builds/latest/amp-windows-amd64.exe](https://repo.amphitheatre.app/cli/builds/latest/amp-windows-amd64.exe)

### Scoop

Amphitheatre CLI can be installed using the Scoop package manager from the
extras bucket. This package is not maintained by the Amphitheatre team.

```
scoop bucket add extras
scoop install amp
```

### Chocolatey

Amphitheatre CLI can be installed using the Chocolatey package manager. This
package is not maintained by the Amphitheatre team.

> **Caution**\
Chocolatey’s installation mechanism interferes with Ctrl+C handling and prevents
amp from cleaning up deployments. This cannot be fixed by Amphitheatre CLI. For
more information about this defect see chocolatey/shimgen#32.

```
choco install -y amp
```

{{ end() }}

{{ pane(tab="os", index=4) }}

For the latest stable release, you can use:

```
docker run amphitheatre/amp:latest amp <command>
```

### Bleeding edge binary

For the latest bleeding edge build:

```
docker run amphitheatre/amp:edge amp <command>
```

{{ end() }}

## Verify

After you install Amphitheatre CLI, verify that you installed it correctly with:

```
amp version
```

## Troubleshooting

If you have any trouble installing Amphitheatre CLI, look for the error message
in the Troubleshooting FAQ.