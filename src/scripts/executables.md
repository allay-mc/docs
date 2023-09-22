# Running Executables

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
as environment variables use a shell script (with [conditions](./conditional.md) or
some other scripting language as a wrapper instead.

Executables are usually installed through some package manager like
[pip](https://pip.pypa.io/en/stable/) or [Cargo](https://doc.rust-lang.org/cargo/).
They may be in text or binary format. Because executables in binary format are not
cross platform, they are either used with a wrapper and [conditions](./conditional.md)
or are present somewhere outside the project's directory. In that case it is good
practise to describe how the executable can be installed.
