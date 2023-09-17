# Scripts

While the official scripts are entirely written in Ruby doesn't mean you cannot
use some other programming language. In fact, you can use any programming language.

It's generally preferred to use scripting languages such as
[Lua](https://www.lua.org/), [Python](https://www.python.org/) or
[Ruby](https://www.ruby-lang.org/). They all don't require a compilation step and
can be run directly independet from the operating system and architecture.

Consider checking out the [official scripts](https://github.com/allay-mc/scripts)
to see how scripts are structured and how they function.


## Examples

### TOML to JSON

```python
#!/usr/bin/env python3

import sys
assert sys.version_info >= (3, 11), "tomllib is only available in Python 3.11 and above"

import json
import os
from pathlib import Path
import tomllib

BP = Path(os.environ["ALLAY_BP_PATH"])
RP = Path(os.environ["ALLAY_RP_PATH"])
SP = Path(os.environ["ALLAY_SP_PATH"])
WT = Path(os.environ["ALLAY_WT_PATH"])

paths = [BP, RP, SP, WT]
for path in paths:
    for p in path.rglob("*.toml"):
        if p.is_file():
            # read TOML file
            with p.open("rb") as f:
                data = tomllib.load(f)

            # remove TOML file
            p.unlink()

            # write JSON file
            with p.with_suffix("json") as f:
                json.dump(f, data)
```


### auto-import add-on

This example requires the environment variable `MINECRAFT_FOLDER` to set to the
location of the Minecraft data:

- Windows: `%localappdata%\Packages\Microsoft.MinecraftUWP_8wekyb3d8bbwe\LocalState\games\com.mojang`
- Android: `storage/emulated/0/Android/data/com.mojang.minecraftpe/files/games/com.mojang`
- iOS & iPadOS: `On My iPhone/Minecraft/games/com.mojang`

```python
#!/usr/bin/env python3

import os
from pathlib import Path
import shutil

TODO
```