+++
title = "The Manifest Format" 
weight = 1 
+++

The `.amp.toml` file for each charactor is called its *manifest*. It is written
in the [TOML] format. Every manifest file consists of the following sections:

* [`[charactor]`](#the-charactor-section) — Defines a charactor.
  * [`name`](#the-name-field) — The name of the charactor.
  * [`version`](#the-version-field) — The version of the charactor.
  * [`authors`](#the-authors-field) — The authors of the charactor.
  * [`description`](#the-description-field) — A description of the charactor.
  * [`readme`](#the-readme-field) — Path to the charactor's README file.
  * [`homepage`](#the-homepage-field) — URL of the charactor homepage.
  * [`repository`](#the-repository-field) — URL of the charactor source repository.
  * [`license`](#the-license-and-license-file-fields) — The charactor license.
  * [`license-file`](#the-license-and-license-file-fields) — Path to the text of the license.
  * [`keywords`](#the-keywords-field) — Keywords for the charactor.
  * [`categories`](#the-categories-field) — Categories of the charactor.
  * [`exclude`](#the-exclude-and-include-fields) — Files to exclude when publishing.
  * [`include`](#the-exclude-and-include-fields) — Files to include when publishing.
  * [`publish`](#the-publish-field) — Can be used to prevent publishing the charactor.
* Partner tables:
  * [`[partners]`](specifying-partners) — Partner dependencies.

### The `[charactor]` section

The first section in a `.amp.toml` is `[charactor]`.

```toml
[charactor]
name = "hello_world" # the name of the charactor
version = "0.1.0"    # the current version, obeying semver
authors = ["Alice <a@example.com>", "Bob <b@example.com>"]
```

The only fields required by Amphitheatre are [`name`](#the-name-field) and
[`version`](#the-version-field). If publishing to a registry, the registry may
require additional fields. See the notes below and [the publishing
chapter][publishing] for requirements for publishing to [Registry].

#### The `name` field

The charactor name is an identifier used to refer to the charactor. It is used
when listed as a partner in another charactor, and as the default name of inferred target.

The name must use only alphanumeric characters or `-` or `_`, and cannot be empty.
Note that [`amp init`] impose some additional restrictions on
the charactor name, such as enforcing that it is a valid Rust identifier and not
a keyword. [Registry] imposes even more restrictions, such as
enforcing only ASCII characters, not a reserved name, not a special Windows
name such as "nul", is not too long, etc.

#### The `version` field

Amphitheatre bakes in the concept of [Semantic
Versioning](https://semver.org/), so make sure you follow some basic rules:
* Use version numbers with three numeric parts such as 1.0.0 rather than 1.0.

#### The `authors` field

The optional `authors` field lists people or organizations that are considered
the "authors" of the charactor. The exact meaning is open to interpretation — it
may list the original or primary authors, current maintainers, or owners of the
charactor. An optional email address may be included within angled brackets at
the end of each author entry.

> **Warning**: Charactor manifests cannot be changed once published, so this
> field cannot be changed or removed in already-published versions of a
> charactor.

#### The `description` field

The description is a short blurb about the charactor. [Registry] will display
this with your charactor. This should be plain text (not Markdown).

```toml
[charactor]
# ...
description = "A short description of my charactor"
```

> **Note**: [Registry] requires the `description` to be set.

#### The `readme` field

The `readme` field should be the path to a file in the charactor root (relative
to this `.amp.toml`) that contains general information about the charactor.
This file will be transferred to the registry when you publish. [Registry]
will interpret it as Markdown and render it on the charactor's page.

```toml
[charactor]
# ...
readme = "README.md"
```

If no value is specified for this field, and a file named `README.md`,
`README.txt` or `README` exists in the charactor root, then the name of that
file will be used. You can suppress this behavior by setting this field to
`false`. If the field is set to `true`, a default value of `README.md` will
be assumed.

#### The `homepage` field

The `homepage` field should be a URL to a site that is the home page for your
charactor.

```toml
[charactor]
# ...
homepage = "https://example.com/"
```

#### The `repository` field

The `repository` field should be a URL to the source repository for your
charactor.

```toml
[charactor]
# ...
repository = "https://github.com/amphitheatre-app/amp-example-go/"
```

#### The `license` and `license-file` fields

The `license` field contains the name of the software license that the charactor
is released under. The `license-file` field contains the path to a file
containing the text of the license (relative to this `.amp.toml`).

[Registry] interprets the `license` field as an [SPDX 2.1 license
expression][spdx-2.1-license-expressions]. The name must be a known license
from the [SPDX license list 3.11][spdx-license-list-3.11]. Parentheses are not
currently supported. See the [SPDX site] for more information.

SPDX license expressions support AND and OR operators to combine multiple
licenses.

```toml
[charactor]
# ...
license = "MIT OR Apache-2.0"
```

Using `OR` indicates the user may choose either license. Using `AND` indicates
the user must comply with both licenses simultaneously. The `WITH` operator
indicates a license with a special exception. Some examples:

* `MIT OR Apache-2.0`
* `LGPL-2.1-only AND MIT AND BSD-2-Clause`
* `GPL-2.0-or-later WITH Bison-exception-2.2`

If a charactor is using a nonstandard license, then the `license-file` field may
be specified in lieu of the `license` field.

```toml
[charactor]
# ...
license-file = "LICENSE.txt"
```

> **Note**: [Registry] requires either `license` or `license-file` to be set.

#### The `keywords` field

The `keywords` field is an array of strings that describe this charactor. This
can help when searching for the charactor on a registry, and you may choose any
words that would help someone find this charactor.

```toml
[charactor]
# ...
keywords = ["gamedev", "graphics"]
```

> **Note**: [Registry] has a maximum of 5 keywords. Each keyword must be
> ASCII text, start with a letter, and only contain letters, numbers, `_` or
> `-`, and have at most 20 characters.

#### The `categories` field

The `categories` field is an array of strings of the categories this charactor
belongs to.

```toml
categories = ["command-line-utilities", "development-tools::build-plugins"]
```

> **Note**: [Registry] has a maximum of 5 categories. Each category should
> match one of the strings available at <https://registry.amphitheatre.app/category_slugs>, and
> must match exactly.

#### The `exclude` and `include` fields

The `exclude` and `include` fields can be used to explicitly specify which
files are included when packaging a charactor to be [published][publishing],
and certain kinds of change tracking (described below).
The patterns specified in the `exclude` field identify a set of files that are
not included, and the patterns in `include` specify files that are explicitly
included.

```toml
[charactor]
# ...
exclude = ["/ci", "images/", ".*"]
```

```toml
[charactor]
# ...
include = ["/src", "COPYRIGHT", "/examples", "!/examples/big_example"]
```

The default if neither field is specified is to include all files from the
root of the charactor, except for the exclusions listed below.

If `include` is not specified, then the following files will be excluded:

* If the charactor is not in a git repository, all "hidden" files starting with
  a dot will be skipped.
* If the charactor is in a git repository, any files that are ignored by the
  [gitignore] rules of the repository and global git configuration will be
  skipped.

Regardless of whether `exclude` or `include` is specified, the following files
are always excluded:

* Any sub-charactor will be skipped (any subdirectory that contains a
  `.amp.toml` file).
* A directory named `target` in the root of the charactor will be skipped.

The following files are always included:

* The `.amp.toml` file of the charactor itself is always included, it does not
  need to be listed in `include`.
* A minimized `.amp.playbook` is automatically included if the charactor contains a
  binary or example target.
* If a [`license-file`](#the-license-and-license-file-fields) is specified, it
  is always included.

The options are mutually exclusive; setting `include` will override an
`exclude`. If you need to have exclusions to a set of `include` files, use the
`!` operator described below.

The patterns should be [gitignore]-style patterns. Briefly:

- `foo` matches any file or directory with the name `foo` anywhere in the
  charactor. This is equivalent to the pattern `**/foo`.
- `/foo` matches any file or directory with the name `foo` only in the root of
  the charactor.
- `foo/` matches any *directory* with the name `foo` anywhere in the charactor.
- Common glob patterns like `*`, `?`, and `[]` are supported:
  - `*` matches zero or more characters except `/`.  For example, `*.html`
    matches any file or directory with the `.html` extension anywhere in the
    charactor.
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

The `publish` field can be used to prevent a charactor from being published to a
charactor registry (like *[Registry]*) by mistake, for instance to keep a charactor
private in a company.

```toml
[charactor]
# ...
publish = false
```

The value may also be an array of strings which are registry names that are
allowed to be published to.

```toml
[charactor]
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
