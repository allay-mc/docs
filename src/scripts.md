# Scripts

<!-- toc -->

Each script is a program run either before building the add-on(s)
(pre-built state) or after that (post-build state). Scripts can for instance be
used to convert between file types such as [JSON5](https://json5.org/) to
[JSON](https://www.json.org/json-en.html) which then can be read by Minecraft.

Scripts are executed in order how they are specified in the configuration file.

```toml
[scripts]
base-path = "scripts/"
pre = [
    { run = "foo.rb", with = "ruby" },  # executed first
    { run = "bar.py", with = "python" },  # executed second
]
```

Always keep this in mind when adding new scripts.

Scripts are by convention located in the `scripts/` directory but you can choose
another directory if you want to. The base path should be specified in the
`base-path` field.

Scripts can also be nested within that directory. Considering the following project
structure:

```text
project
├── allay.toml
├── build/
├── pack_icon.png
├── scripts/
│   ├── converters/
│   │   ├── yaml_to_json.rb
│   │   └── toml_to_json.rb
│   └── linters/
│       └── yaml.py
└── src/
```

Then you can refer to the scripts like this in your configuration file.

```toml
[scripts]
base-path = "scripts"
pre = [
    { run = "linters/yaml.py", with = "python" },
    { run = "converters/yaml_to_json.rb", with = "ruby" },
    { run = "converters/toml_to_json.rb", with = "ruby" },
]
post = []
```

There are some scripts provided in the
[allay-mc/scripts](https://github.com/allay-mc/scripts/) repository. They are
entirely written in [Ruby](https://www.ruby-lang.org/en/) without any dependencies.
Just make sure you have Ruby installed when using scripts from there.

```admonish tip
You can use the `allay add` command to quickly download scripts from that
repository. For more info, type `allay add --help` in the command-line.
```


## Additional Options

Some scripts might accept additional options which can be set with the `args`
field.

```toml
[scripts]
base-path = "scripts/"
pre = [
    {
        run = "templating.rb",
        with = "ruby",
        args = [
            "--trim-mode=%"  # enables Ruby code processing for lines beginning with `%`
        ],
    }
]
```

Note that arguments are not pre-processed like shells do meaning you cannot use
[glob patterns](https://en.wikipedia.org/wiki/Glob_(programming)) or environment
variables.

```toml
[scripts]
base-path = "scripts/"
pre = [
    {
        run = "templating.rb",
        with = "ruby",
        args = [
            "--trim-mode=$ERB_TRIM_MODE"  # `$ERB_TRIM_MODE` will be passed to the program as is
        ]
    }
]
```

A way to achive that is by modifying the source code or wrapping the program with
another program. The latter one may be done with a scripting language or a shell
script which need to be used with caution explained in the next section.

```admonish todo
how to add third-party scripts
```

```admonish danger
**Always make sure** the scripts you add to your project are safe and don't
execute **harmful code**.
```

Post scripts are less common because they work with the finalized built add-on.
Ususally post-scripts only move the add-on to another location in the file
system or upload the add-on.


## Conditional Scripts

There are often cases where one want to execute simple commands such as
[`tsc`](https://www.typescriptlang.org/docs/handbook/compiler-options.html) to
transpile TypeScript along with dynamic arguments such as environment variables.

```toml
[scripts]
base-path = "scripts/"
pre = [
    {
        run = "ts_to_js.sh",
        with = "sh",
    }
]
```

```bash
#!/usr/bin/env bash
shopt -s globstar         # allow recursive glob patterns
tsc $ALLAY_PREBUILD/*.ts  # transpile to JavaScript
rm **/*.ts                # remove TypeScript files (from prebuild)
```

However this cannot be used from other operating systems than Linux such as Windows
as they do not have the `sh` shell. In order to make it compatible with Windows,
the same script needs to be written in a supported language such as PowerShell:

```powershell
tsc $Env:ALLAY_PREBUILD/*.ts                          # transpile to JavaScript
Get-ChildItem * -Include *.ts -Recurse | Remove-Item  # remove TypeScript files (from prebuild)
```

The configuration would then look like this:

```toml
[scripts]
base-path = "scripts/"
pre = [
    {
        run = "ts_to_js.sh",
        with = "bash",
        when = "os = 'linux'",
    },
    {
        run = "ts_to_js.ps1",
        with = "powershell",
        when = "os = 'windows'",
    }
]
```

### Options

#### `arch`

Possible values:

- `'x86'`
- `'x86_64'`
- `'mips'`
- `'powerpc'`
- `'powerpc64'`
- `'arm'`
- `'aarch64'`


#### `os`

Possible values:

- `'windows'`
- `'macos'`
- `'ios'`
- `'linux'`
- `'android'`
- `'freebsd'`
- `'dragonfly'`
- `'openbsd'`
- `'netbsd'`

#### `family`

Possible values:

- `'unix'`
- `'windows'`
- `'wasm'`

#### `allay_version`

Possible values:

- `0.1.0-alpha.1`
- `0.1.0`
- ...

### Operators and Grouping

In addition, conditions can be grouped with parenthesis meaning they are evaluated
first or chained and modified with the following operators::

- _condition_ `&` _condition_ --- evaluates `true` when both conditions are met
- _condition_ `|` _condition_ --- evaluates `true` when at least one of the condition is met
- _condition_ `^` _condition_ --- evaluates `true` when exactly one of the condition is met
- `!` _condition_ --- evaluates `true` when the condition is not met (example: `!os = 'linux'`)


## Running executables

```toml
[scripts]
# ...
pre = [
    {
        run = "name-of-executable",
        args = ["arg1", "arg2"]
    }
]
```

This will invoke the `name-of-executable` executable. Note that the executable is not
required to be in the `base-path` directory. If you want to supply dynamic values such
as environment variables use a shell script (with [conditions](#conditional-scripts)) or
some other scripting language as a wrapper instead.

