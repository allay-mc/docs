# Configuration

Some scripts might rely on options that can be configured by the user. Instead of
manually editing constants in the script, one can use the configuartion file.


## Using command-line options

Most programming languages a capable of reading arguments provided in the
command-line. Rust uses these method to allow users to provide options
specific to a script in the configuration file.

```python
# FILE: helloworld.py
from argparse import ArgumentParser

parser = ArgumentParser()
parser.add_argument("name")
parser.add_argument("age", type=int)
args = parser.parse_args()
print(f"You are {args.name} and you are {arg.age} years old.")
```

One can then provide arguments in their configuration file like this:

```toml
[scripts]
# ...
pre = [
  { run = "helloworld.py", with = "python", args = ["John Doe", "42"] }
]
```

Remember to document the arguments somewhere.


### Parsing Libraries

Language | Library
---------|------------------------------------------------------------------------------
Lua      | [`argparse`](https://github.com/mpeterv/argparse)
NodeJS   | [`parseArg`](https://nodejs.org/api/util.html#utilparseargsconfig) (`std`)
Python   | [`argparse`](https://docs.python.org/3/library/argparse.html) (`std`)
Ruby     | [`optparse`](https://ruby-doc.org/stdlib-2.7.0/libdoc/optparse/rdoc/) (`std`)


## Using `[metadata]`

Another less recommended approach is the usage of the
[`[metadata]`](../reference/configuration.md#metadat) section in the configuration
file. It is designed to be used by external programs but parsing TOML is less
supported than parsing command-line arguments in many programming languages.

```python
# FILE: helloworld.py

import os
from pathlib import Path
import tomllib

config_file = Path(os.environ["ALLAY_CONFIG"])
with config_file.open("rb") as f:
    data = tomllib.load(f)["metadata"]["helloworld"]

print(f"You are {data['name']} and you are {data['age']} years old.")
```

One can then provide arguments in their configuration file like this:

```toml
[metadata.helloworld]
name = "John Doe"
age = 42
```

