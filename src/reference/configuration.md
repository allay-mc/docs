# Configuration

An Allay project only exists if an `allay.toml` file is present. It contains
several configuration options for the project and can be used to configure
scripts as well.

- [`[project.name]`](#the-projectname-section)
- [`[project.description]`](#the-projectdescription-section)
- [`[project]`](#the-project-section)
  - [`version`](#the-version-field)
  - [`development`](#the-development-field)
  - [`min-engine-version`](#the-min-engine-version-field)
  - [`authors`](#the-authors-field)
  - [`license`](#the-license-field)
  - [`url`](#the-url-field)
- [`localization`](#the-localization-section)
  - [`primary-language`](#the-primary-language-field)
  - [`localization-groups`](#the-localization-groups-field)
- [`scripts`](#the-scripts-section)
  - [`base-path`](#the-base-path-field)
  - [`pre`](#the-pre-field)
  - [`post`](#the-post-field)

--------------------------------------------------------------------------------

## The `[project.name]` section

The name of the project which will be used for the individual add-ons as well
unless overridden with `pack.*.name` where `*` is one of `bp`, `rp`, `sp` or `wt`.

```toml
[project.name]
de-de = "Hallo Welt"
en-us = "Hello World"
```

## The `[project.description]` section

The description of the project which will be used for the individual add-ons as
well unless overridden with `pack.*.description` where `*` is one of `bp`, `rp`,
`sp` or `wt`.

```toml
[project.description]
de-de = "Das ist mein tolles Projekt."
en-us = "This is my amazing project."
```

## The `[project]` section

### The `version` field

The version of the project of the form `n.n.n` where `n` is any natural number or
zero.

```toml
[project]
# ...
version = "0.1.0"
```


### The `development` field

Specifies whether the project is currently in development. This has impact on the
build process and maybe some scripts. This behavior can be overridden with
`allay build --release`.

```toml
[project]
# ...
development = true
```


### The `min-engine-version` field

> This is the minimum version of the game that this pack was written for. This is a
> required field for resource and behavior packs. This helps the game identify
> whether any backwards compatibility is needed for your pack. You should always use
> the highest version currently available when creating packs.
>
> --- from <https://learn.microsoft.com/en-us/minecraft/creator/reference/content/addonsreference/examples/addonmanifest>

### The `authors` field

An array of people that are considered the "authors" of the project. An optional
email address may be included within angled brackets at the end of each author
entry.

```toml
[project]
# ...
authors = ["John Doe", "Joana Doe <joana.doe@example.org>"]
```


### The `license` field

The name of the license(s). You might want to include a `LICENSE.txt` file in the
root of your project which contains the content of the license.

```toml
[project]
# ...
license = "MIT OR Apache-2.0"
```


### The `url` field

The URL to a site that is the home page for your project.

```toml
[project]
# ...
url = "https://github.com/allay-mc/example"
```


## The `[localization]` section

### The `primary-language` field

### The `localization-groups` field

## The `[scripts]` section

### The `base-path` field

### The `pre` field

### The `post` field
