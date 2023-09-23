# Conditional Scripts

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

## Options

### `arch`

Possible values:

- `'x86'`
- `'x86_64'`
- `'mips'`
- `'powerpc'`
- `'powerpc64'`
- `'arm'`
- `'aarch64'`


### `os`

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

### `family`

Possible values:

- `'unix'`
- `'windows'`
- `'wasm'`

### `allay_version`

Possible values:

- `0.1.0-alpha.1`
- `0.1.0`
- ...

## Operators and Grouping

In addition, conditions can be grouped with parenthesis meaning they are evaluated
first or chained and modified with the following operators:

- _condition_ `&` _condition_ --- evaluates `true` when both conditions are met
- _condition_ `|` _condition_ --- evaluates `true` when at least one of the condition is met
- _condition_ `^` _condition_ --- evaluates `true` when exactly one of the condition is met
- `!` _condition_ --- evaluates `true` when the condition is not met (example: `!os = 'linux'`)

Conditions can be wrapped in parentheses to group them:

```
(condition1 & condition2) | condition3
```

This would require both condition1 and condition2 to evaluate true or just condition3.

