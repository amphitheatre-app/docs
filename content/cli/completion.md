+++
title = "amp completion"
description = "Output shell completion for the given shell (bash or zsh)"
weight = 1
+++

## Usage
```
amp completion [OPTIONS] <SHELL>
```

## Arguments

* `<SHELL>`  possible values: `bash`, `elvish`, `fish`, `powershell`, `zsh`

> Use "amp options" for a list of global command-line options (applies to all commands).

When installing Amphitheatre CLI through a package manager, it's possible that no
additional shell configuration is necessary to gain completion support. For
Homebrew, see [https://docs.brew.sh/Shell-Completion](https://docs.brew.sh/Shell-Completion)

If you need to set up completions manually, follow the instructions below. The
exact config file locations might vary based on your system. Make sure to
restart your shell before testing whether completions are working.

## bash

First, ensure that you install bash-completion using your package manager.
After, add this to your `~/.bash_profile`:

```
eval "$(amp completion bash)"
```

## zsh

Generate a `_amp` completion script and put it somewhere in your `$fpath`:

```
amp completion zsh > /usr/local/share/zsh/site-functions/_amp
```

Ensure that the following is present in your `~/.zshrc`:

```
autoload -U compinit
compinit -i
```

Zsh version 5.7 or later is recommended.

## fish

Generate a `amp.fish` completion script:

```
amp completion fish > ~/.config/fish/completions/amp.fish
```

## PowerShell

Open your profile script with:

```
mkdir -Path (Split-Path -Parent $profile) -ErrorAction SilentlyContinue
notepad $profile
```

Add the line and save the file:

```
Invoke-Expression -Command $(amp completion powershell | Out-String)
```
