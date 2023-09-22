# Scripts

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

Post scripts are less common because they work with the finalized built add-on.
Ususally post-scripts only move the add-on to another location in the file
system or upload the add-on.
