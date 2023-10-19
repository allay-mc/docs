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
- [`[localization]`](#the-localization-section)
  - [`primary-language`](#the-primary-language-field)
  - [`localization-groups`](#the-localization-groups-field)
- [`[scripts]`](#the-scripts-section)
  - [`base-path`](#the-base-path-field)
  - [`pre`](#the-pre-field)
  - [`post`](#the-post-field)
- [`[build]`](#the-build-section)
  - [`include`](#the-include-and-exclude-field)
  - [`exclude`](#the-include-and-exclude-field)
  - [`use-custom-manifest`](#the-use-custom-manifest-field)

--------------------------------------------------------------------------------

## The `[project.name]` section

The name of the project which will be used for the individual add-ons as well
unless overridden with `pack.*.name` where `*` is one of `bp`, `rp`, `sp` or `wt`.

```toml,icon=gear,filepath=allay.toml
[project.name]
de-de = "Hallo Welt"
en-us = "Hello World"
```

## The `[project.description]` section

The description of the project which will be used for the individual add-ons as
well unless overridden with `pack.*.description` where `*` is one of `bp`, `rp`,
`sp` or `wt`.

```toml,icon=gear,filepath=allay.toml
[project.description]
de-de = "Das ist mein tolles Projekt."
en-us = "This is my amazing project."
```

## The `[project]` section

### The `version` field

The version of the project of the form `n.n.n` where `n` is any natural number or
zero.

```toml,icon=gear,filepath=allay.toml
[project]
# ...
version = "0.1.0"
```


### The `development` field

Specifies whether the project is currently in development. This has impact on the
build process and maybe some scripts. This behavior can be overridden with
`allay build --release`.

```toml,icon=gear,filepath=allay.toml
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

```toml,icon=gear,filepath=allay.toml
[project]
# ...
authors = ["John Doe", "Joana Doe <joana.doe@example.org>"]
```


### The `license` field

The name of the license(s). You might want to include a `LICENSE.txt` file in the
root of your project which contains the content of the license.

```toml,icon=gear,filepath=allay.toml
[project]
# ...
license = "MIT OR Apache-2.0"
```


### The `url` field

The URL to a site that is the home page for your project.

```toml,icon=gear,filepath=allay.toml
[project]
# ...
url = "https://github.com/allay-mc/example"
```


## The `[localization]` section

### The `primary-language` field

The primary language the add-on use. This is the language that untranslated
languages will ultimatively fall back to.

```toml,icon=gear,filepath=allay.toml
[localization]
primary-language = "en-us"
```

### The `localization-groups` field

Using this field allows you to override existing language groups or create new
ones. This is explained in detail in [chapter 4](../localitaion.md)

```toml,icon=gear,filepath=allay.toml
[localization]
localization-groups = [
  ["ru-ru", "uk-ua"], 
  # puts russian and ukrainian in the same group meaning if a translation for
  # russian exists, ukrainian will fall back to that and vice versa

  ["de-de", "de-at"]
  # puts german german and austrian german, which is not supported natively but
  # could be implemented with a resource pack, in one group
]
```

## The `[scripts]` section

### The `base-path` field

This is the path relative to the project's root directory (the directory that
contains the `allay.toml` file) where scripts should be searched for. This is
the `scripts` directory by convention but you are free to choose whatever
directory you want.

### The `pre` field

An array of scripts run before building the add-on. This is explained in detail
in [chapter 3](../scripts/index.md)

### The `post` field

An array of scripts run afte the add-ons have been built. This is explained in
detail in [chapter 3](../scripts/index.md)


## The `[build]` section

### The `include` and `exclude` field

These fields allow you to specify glob patterns which affect the presence of
specific files in the built add-on.

```toml,icon=gear,filepath=allay.toml
include = [
  "**/*.json",
  "**/*.md",
  "**/*.js",
  "**/*.png",
  "**/*.mcfunction",
  "**/*.mcstructure",
  "**/*.lang",
]
```

The configuration above makes Allay remove all files except the ones that have
one of the above mentioned file extensions. Using this field is generally
discouraged because an add-on uses several different files with different names
and extensions. Minecraft will not complain if the add-on contains unrecognized
files. It should be the job of scripts which convert files that are not used in
the final build to remove those files.

Using the `exclude` file on the other hand is acceptable if a script for example
does not remove unused files or if you just want to include notices in the source
but remove them in the build.

```toml,icon=gear,filepath=allay.toml
exclude = ["**/LICENSE.txt", "**/README.md"]
```

The above example keeps all files except those named `LICENSE.txt` and `README.md`.

```admonish warning
You cannot set both `include` and `exclude` at the same time.
```

```admonish important
The `manifest.json` file is the only file that is kept no matter what patterns are
specified in `include` and `exclude`.
```

### The `use-custom-manifest` field

Because the `allay.toml` removes the need of a `manifest.json` file, Allay will
display a warning if it found such file and ignores it. If you, for some reason
need to use a custom `manifest.json`, then set this option to `true`.
