+++
title = "Installing CLI"
description = "Install the Amphitheatre Command Line Tool."
weight = 4

[extra]
toc = false
+++

Amphitheatre CLI can be installed by using various package managers on Linux, macOS and Windows.
The precompiled binaries are available from the [**GitHub releases page**](https://github.com/amphitheatre-app/cli/releases).
Simply download the appropriate binary and add it to your `PATH`.

For detailed install instructions, follow the guide for your operating system.

{{ tabs(id="os", items=["Linux", "macOS", "Windows", "Docker"])}}

{{ pane(tab="os", index=1) }}

Copy and paste one of the following commands in your terminal:

```
# For Linux on x86_64 (amd64)
curl -L https://github.com/amphitheatre-app/cli/releases/download/latest/amp-linux-amd64.tar.gz && \
tar -xzf amp-linux-amd64.tar.gz && \
sudo install amp /usr/local/bin/
```

```
# For Linux on aarch64 (arm64)
curl -L https://github.com/amphitheatre-app/cli/releases/download/latest/amp-linux-arm64.tar.gz && \
tar -xzf amp-linux-arm64.tar.gz && \
sudo install amp /usr/local/bin/
```

{{ end() }}

{{ pane(tab="os", index=2) }}

Copy and paste one of the following commands in your terminal:

```
# For macOS on x86_64 (amd64)
curl -L https://github.com/amphitheatre-app/cli/releases/download/latest/amp-darwin-amd64.tar.gz && \
tar -xzf amp-darwin-amd64.tar.gz && \
sudo install amp /usr/local/bin/
```

```
# For macOS on aarch64 (arm64)
curl -L https://github.com/amphitheatre-app/cli/releases/download/latest/amp-darwin-arm64.tar.gz && \
tar -xzf amp-darwin-arm64.tar.gz && \
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

1. First, visit the following link to download the file:

    [https://github.com/amphitheatre-app/cli/releases/download/latest/amp-windows-amd64.zip](https://github.com/amphitheatre-app/cli/releases/download/latest/amp-windows-amd64.zip)

    Click on the link, and your browser will begin downloading the compressed file (`amp-windows-amd64.zip`).

2. Once the download is complete, you can extract the file to a directory of
    your choice. You can do this using the built-in decompression tool of your
    operating system or a third-party compression tool like 7-Zip or WinRAR.

3. After extraction, you will have an executable file named `amp.exe``.

4. Finally, add the `amp.exe` file to your `PATH` environment variable so that
   you can conveniently run the `amp` command from any location.

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
docker run ghcr.io/amphitheatre-app/amp:latest <command>
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
