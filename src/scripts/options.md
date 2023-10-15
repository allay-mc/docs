# Additional Options

Some scripts might accept additional options which can be set with the `args`
field.

```toml,icon=gear,filepath=allay.toml
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

All arguments must be of type string even if the script expects a different type.
In reality the string is converted by the script just like it would be provided in
the command-line. Note that arguments are not pre-processed like shells do meaning
you cannot use [glob patterns](https://en.wikipedia.org/wiki/Glob_(programming)) or
environment variables.

```toml,icon=gear,filepath=allay.toml
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
another program which is discussed in the next chapter.

