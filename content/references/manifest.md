+++
title = "The Manifest Format" 
weight = 1 
+++

The `.amp.toml` file for each character is called its *manifest*. It is written
in the [TOML] format. Every manifest file consists of the following sections:

* [`[character]`](#the-character-section) — Defines a character.
  * [`name`](#the-name-field) — The name of the character.
  * [`version`](#the-version-field) — The version of the character.
  * [`authors`](#the-authors-field) — The authors of the character.
  * [`description`](#the-description-field) — A description of the character.
  * [`readme`](#the-readme-field) — Path to the character's README file.
  * [`homepage`](#the-homepage-field) — URL of the character homepage.
  * [`repository`](#the-repository-field) — URL of the character source repository.
  * [`license`](#the-license-and-license-file-fields) — The character license.
  * [`license-file`](#the-license-and-license-file-fields) — Path to the text of the license.
  * [`keywords`](#the-keywords-field) — Keywords for the character.
  * [`categories`](#the-categories-field) — Categories of the character.
  * [`exclude`](#the-exclude-and-include-fields) — Files to exclude when publishing.
  * [`include`](#the-exclude-and-include-fields) — Files to include when publishing.
  * [`publish`](#the-publish-field) — Can be used to prevent publishing the character.
* Partner tables:
  * [`[partners]`](specifying-partners) — Partner dependencies.

### The `[character]` section

The first section in a `.amp.toml` is `[character]`.

```toml
[character]
name = "hello_world" # the name of the character
version = "0.1.0"    # the current version, obeying semver
authors = ["Alice <a@example.com>", "Bob <b@example.com>"]
```

The only fields required by Amphitheatre are [`name`](#the-name-field) and
[`version`](#the-version-field). If publishing to a registry, the registry may
require additional fields. See the notes below and [the publishing
chapter][publishing] for requirements for publishing to [Registry].

#### The `name` field

The character name is an identifier used to refer to the character. It is used
when listed as a partner in another character, and as the default name of inferred target.

The name must use only alphanumeric characters or `-` or `_`, and cannot be empty.
Note that [`amp init`] impose some additional restrictions on
the character name, such as enforcing that it is a valid Rust identifier and not
a keyword. [Registry] imposes even more restrictions, such as
enforcing only ASCII characters, not a reserved name, not a special Windows
name such as "nul", is not too long, etc.

#### The `version` field

Amphitheatre bakes in the concept of [Semantic
Versioning](https://semver.org/), so make sure you follow some basic rules:
* Use version numbers with three numeric parts such as 1.0.0 rather than 1.0.

#### The `authors` field

The optional `authors` field lists people or organizations that are considered
the "authors" of the character. The exact meaning is open to interpretation — it
may list the original or primary authors, current maintainers, or owners of the
character. An optional email address may be included within angled brackets at
the end of each author entry.

> **Warning**: Character manifests cannot be changed once published, so this
> field cannot be changed or removed in already-published versions of a
> character.

#### The `description` field

The description is a short blurb about the character. [Registry] will display
this with your character. This should be plain text (not Markdown).

```toml
[character]
# ...
description = "A short description of my character"
```

> **Note**: [Registry] requires the `description` to be set.

#### The `readme` field

The `readme` field should be the path to a file in the character root (relative
to this `.amp.toml`) that contains general information about the character.
This file will be transferred to the registry when you publish. [Registry]
will interpret it as Markdown and render it on the character's page.

```toml
[character]
# ...
readme = "README.md"
```

If no value is specified for this field, and a file named `README.md`,
`README.txt` or `README` exists in the character root, then the name of that
file will be used. You can suppress this behavior by setting this field to
`false`. If the field is set to `true`, a default value of `README.md` will
be assumed.

#### The `homepage` field

The `homepage` field should be a URL to a site that is the home page for your
character.

```toml
[character]
# ...
homepage = "https://example.com/"
```

#### The `repository` field

The `repository` field should be a URL to the source repository for your
character.

```toml
[character]
# ...
repository = "https://github.com/amphitheatre-app/amp-example-go/"
```

#### The `license` and `license-file` fields

The `license` field contains the name of the software license that the character
is released under. The `license-file` field contains the path to a file
containing the text of the license (relative to this `.amp.toml`).

[Registry] interprets the `license` field as an [SPDX 2.1 license
expression][spdx-2.1-license-expressions]. The name must be a known license
from the [SPDX license list 3.11][spdx-license-list-3.11]. Parentheses are not
currently supported. See the [SPDX site] for more information.

SPDX license expressions support AND and OR operators to combine multiple
licenses.

```toml
[character]
# ...
license = "MIT OR Apache-2.0"
```

Using `OR` indicates the user may choose either license. Using `AND` indicates
the user must comply with both licenses simultaneously. The `WITH` operator
indicates a license with a special exception. Some examples:

* `MIT OR Apache-2.0`
* `LGPL-2.1-only AND MIT AND BSD-2-Clause`
* `GPL-2.0-or-later WITH Bison-exception-2.2`

If a character is using a nonstandard license, then the `license-file` field may
be specified in lieu of the `license` field.

```toml
[character]
# ...
license-file = "LICENSE.txt"
```

> **Note**: [Registry] requires either `license` or `license-file` to be set.

#### The `keywords` field

The `keywords` field is an array of strings that describe this character. This
can help when searching for the character on a registry, and you may choose any
words that would help someone find this character.

```toml
[character]
# ...
keywords = ["gamedev", "graphics"]
```

> **Note**: [Registry] has a maximum of 5 keywords. Each keyword must be
> ASCII text, start with a letter, and only contain letters, numbers, `_` or
> `-`, and have at most 20 characters.

#### The `categories` field

The `categories` field is an array of strings of the categories this character
belongs to.

```toml
categories = ["command-line-utilities", "development-tools::build-plugins"]
```

> **Note**: [Registry] has a maximum of 5 categories. Each category should
> match one of the strings available at <https://registry.amphitheatre.app/category_slugs>, and
> must match exactly.

#### The `exclude` and `include` fields

The `exclude` and `include` fields can be used to explicitly specify which
files are included when packaging a character to be [published][publishing],
and certain kinds of change tracking (described below).
The patterns specified in the `exclude` field identify a set of files that are
not included, and the patterns in `include` specify files that are explicitly
included.

```toml
[character]
# ...
exclude = ["/ci", "images/", ".*"]
```

```toml
[character]
# ...
include = ["/src", "COPYRIGHT", "/examples", "!/examples/big_example"]
```

The default if neither field is specified is to include all files from the
root of the character, except for the exclusions listed below.

If `include` is not specified, then the following files will be excluded:

* If the character is not in a git repository, all "hidden" files starting with
  a dot will be skipped.
* If the character is in a git repository, any files that are ignored by the
  [gitignore] rules of the repository and global git configuration will be
  skipped.

Regardless of whether `exclude` or `include` is specified, the following files
are always excluded:

* Any sub-character will be skipped (any subdirectory that contains a
  `.amp.toml` file).
* A directory named `target` in the root of the character will be skipped.

The following files are always included:

* The `.amp.toml` file of the character itself is always included, it does not
  need to be listed in `include`.
* A minimized `.amp.playbook` is automatically included if the character contains a
  binary or example target.
* If a [`license-file`](#the-license-and-license-file-fields) is specified, it
  is always included.

The options are mutually exclusive; setting `include` will override an
`exclude`. If you need to have exclusions to a set of `include` files, use the
`!` operator described below.

The patterns should be [gitignore]-style patterns. Briefly:

- `foo` matches any file or directory with the name `foo` anywhere in the
  character. This is equivalent to the pattern `**/foo`.
- `/foo` matches any file or directory with the name `foo` only in the root of
  the character.
- `foo/` matches any *directory* with the name `foo` anywhere in the character.
- Common glob patterns like `*`, `?`, and `[]` are supported:
  - `*` matches zero or more characters except `/`.  For example, `*.html`
    matches any file or directory with the `.html` extension anywhere in the
    character.
  - `?` matches any character except `/`. For example, `foo?` matches `food`,
    but not `foo`.
  - `[]` allows for matching a range of characters. For example, `[ab]`
    matches either `a` or `b`. `[a-z]` matches letters a through z.
- `**/` prefix matches in any directory. For example, `**/foo/bar` matches the
  file or directory `bar` anywhere that is directly under directory `foo`.
- `/**` suffix matches everything inside. For example, `foo/**` matches all
  files inside directory `foo`, including all files in subdirectories below
  `foo`.
- `/**/` matches zero or more directories. For example, `a/**/b` matches
  `a/b`, `a/x/b`, `a/x/y/b`, and so on.
- `!` prefix negates a pattern. For example, a pattern of `src/*.rs` and
  `!foo.rs` would match all files with the `.rs` extension inside the `src`
  directory, except for any file named `foo.rs`.

[gitignore]: https://git-scm.com/docs/gitignore

#### The `publish` field

The `publish` field can be used to prevent a character from being published to a
character registry (like *[Registry]*) by mistake, for instance to keep a character
private in a company.

```toml
[character]
# ...
publish = false
```

The value may also be an array of strings which are registry names that are
allowed to be published to.

```toml
[character]
# ...
publish = ["some-registry-name"]
```

If publish array contains a single registry, `amp publish` command will use
it when `--registry` flag is not specified.

### Partner sections

See the [specifying partners page](specifying-partners) for
information on the `[partners]` section.

[`amp init`]: @/cli/init.md
[`amp run`]: @/cli/run.md
[registry]: https://registry.amphitheatre.app/
[publishing]: publishing
[spdx-2.1-license-expressions]: https://spdx.org/spdx-specification-21-web-version#h.jxpfx0ykyb60
[spdx-license-list-3.11]: https://github.com/spdx/license-list-data/tree/v3.11
[SPDX site]: https://spdx.org/license-list
[TOML]: https://toml.io/