+++
title = "Installing Amphitheatre Desktop"
description = "Install the Amphitheatre Desktop Application"
weight = 1
+++

Amphitheatre Desktop is lightweight and simple to install on all major
platforms. You can find all installation options on the [Amphitheatre
website](https://amphitheatre.app/).

## System requirements

Before you proceed with Amphitheatre Desktop installation, verify that your
system meets the system requirements.

### Hardware requirements

Minimum hardware requirements:

- 2 GHz or faster processor
- 1 GB of RAM
- 1 GB of disk space

### Platforms

Amphitheatre Desktop has been tested on the following platforms: macOS, Windows
and Linux.

Although Amphitheatre Desktop may still work on operating systems that are
beyond their end of life support date we highly recommend staying on supported
versions of the underlying operating system. Please see a list of supported OS
versions here.

{{ tabs(id="os", items=["macOS", "Windows", "Linux"])}}

{{ pane(tab="os", index=1) }}

- Download Amphitheatre Desktop from the [Amphitheatre
  website](https://amphitheatre.app/) for macOS.
- Double-click amphitheatre-desktop-{version}.dmg and drag
  amphitheatre-desktop.app to the Applications folder, making it available in
  the macOS Launchpad.
- Add Amphitheatre Desktop to your Dock by right-clicking on the icon to bring
  up the context menu and choosing Options, Keep in Dock.

{{ end() }}

{{ pane(tab="os", index=2) }}

- Download the [Amphitheatre Desktop installer](https://amphitheatre.app/) for
  Windows.
- Install Amphitheatre Desktop by running the
  Amphitheatre-Desktop-Setup-{version}.exe installer. By default, Amphitheatre
  Desktop installs under
  C:\users\{username}\AppData\Local\Programs\AmphitheatreDesktop.

{{ end() }}

{{ pane(tab="os", index=3) }}

You have the following downloading options for Linux: `.rpm` and .`deb` files,
`AppImage` file and `.snap` file.

#### Default package formats

This example shows how to install Amphitheatre Desktop on Ubuntu 20.04:

- Download the .deb file from here:
  [https://repo.amphitheatre.app/desktop/releases/latest/amphitheatre-desktop.deb](https://repo.amphitheatre.app/desktop/releases/latest/amphitheatre-desktop.deb).
- In the Linux command line, type the following:

    ```
    sudo apt install <path-to-deb-file>
    ```

    You can also use one of the applications for `.deb` file installations.
    > **Note**\
    For RPM-based distributions, the procedure is similar with slight differencies in terminal commands.

#### AppImage

- Download the AppImage file from
  [https://repo.amphitheatre.app/desktop/releases/latest/amphitheatre-desktop.AppImage](https://repo.amphitheatre.app/desktop/releases/latest/amphitheatre-desktop.AppImage).
- Navigate to the directory where you have the file downloaded and right-click
  the AppImage file and select Properties > Permissions.
- Select the Allow executing file as a program check box.

    > **Note**\
    Some file managers might offer to unselect the Is executable check box or other similar actions.

- In the Linux command line, navigate to the directory where you have the file
  downloaded and type the following commands:

    ```sh
    chmod +x <file-name.AppImage>
    ./<file-name.AppImage>
    ```

After installing Amphitheatre Desktop manually (not using a package manager file
such as .deb or .rpm) the following will need to be done to allow protocol
handling. This assumes that your linux distribution uses xdg-open and the xdg-*
suite of programs for determining which application can handle custom URIs.

- Create a file called amphitheatre-desktop.desktop in one of the following directions:

- ~/.local/share/applications/ to install for all users

- /usr/share/applications to install for your user

- That file should have the following contents, with <path/to/executable> being
  the absolute path to where you have installed the unpacked Amphitheatre
  Desktop executable:

    ```
    [Desktop Entry]
    Name=Amphitheatre Desktop
    Exec=<path/to/executable> %U
    Terminal=false
    Type=Application
    Icon=logo
    StartupWMClass=AmphitheatreDesktop
    Comment=Amphitheatre Desktop - Open source GUI application that enables you to interact with Amphitheatre
    MimeType=x-scheme-handler/amp;
    Categories=Network;
    ```

- Then run the following command:

    ```
    xdg-settings set default-url-scheme-handler amp amphitheatre-desktop.desktop
    ```

- If that succeeds (exits with code 0) then your Amphitheatre Desktop install
  should be set up to handle `amp://` URIs.

#### Snap

- Download the [Amphitheatre Desktop package](https://amphitheatre.app/) for
  Snap.
- In the Linux command line, type the following:

    ```
    sudo snap install amphitheatre-desktop-{version}.amd64.snap --dangerous --classic
    ```

{{ end() }}

## Updates

Amphitheatre Desktop supports auto-updating. When a new release is available, a
notification appears in the application.

> **Note**\
Auto-updates are available only for exe, dmg, and AppImage installations. Update
other distributions manually.

See Amphitheatre Desktop [release
notes](https://github.com/amphitheatre-app/desktop/releases) to find information
about features, maintenance and so on.

## Backup and restore data

When updating to a major Amphitheatre Desktop version, user data such as
clusters, preferences, and so on, migrates to new data structures. However, we
recommend you to back up user data which is by default stored in the following
directions:

- macOS `~/Library/Application Support/AmphitheatreDesktop/`
- Windows `%APPDATA%\AmphitheatreDesktop\`
- Linux `~/.config/AmphitheatreDesktop/` or `$XDG_CONFIG_HOME/AmphitheatreDesktop`

## Troubleshooting

If you have any trouble installing Amphitheatre Desktop, look for the error message
in the Troubleshooting FAQ and its [Github
Issues](https://github.com/amphitheatre-app/desktop/issues).