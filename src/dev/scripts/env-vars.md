# Environment Variables

When Allay runs a script, it will also provide several environment variables that
can be used within the script. Note that these are only set when Allay executes
scripts meaning they are not accessible normally.

> **Example**
>
> ```python
> # FILE: myscript.py
> import os
> os.environ["ALLAY_BP_PATH"]
> ```

- `ALLAY_BP_PATH` --- where the prebuilt behavior pack is located
- `ALLAY_BUILD` --- where the built add-ons are located
- `ALLAY_CONFIG` --- where the configuration file is located
- `ALLAY_PREBUILD` --- where the prebuilt add-ons are located[^1]
- `ALLAY_RP_PATH` --- where the prebuilt resource pack is located
- `ALLAY_RELEASE` --- whether the add-ons are beeing built in release mode
- `ALLAY_SP_PATH` --- where the prebuilt skin pack is located
- `ALLAY_VERSION` --- the version of Allay that is beeing used
- `ALLAY_WT_PATH` --- where the prebuilt world template is located
- `ALLAY_ROOT` --- where the project is located


```admonish attention
Allay overrides existing environment variables during execution.
```

~~~admonish attention
Always use paths provided by Allay's environment variables instead of specifying
them on your own unless the programming languages doesn't support accessing
environment variables.

**Never** modify the original source. Allay copies the directories in `src` into
a seperate directory (`.allay/prebuild/`) which is allowed to be modified. The
environment variables refer to that location.

The reason for this is, that some scripts may be nested inside a directory
containig all scripts. Now if a script tries to access some path relatively
it might break for some users due to their different structure.

```python
# BAD:
from pathlib import Path
path = Path("../.allay/prebuild/BP/")
...
```

```python
# WORSE:
from pathlib import Path
path = Path("../src/BP/")
...
```

```python
# GOOD:
import os
from pathlib import Path
path = Path(os.environ["ALLAY_BP_PATH"])
...
```
~~~

[^1]: Usually the variables `ALLAY_*_PATH` variables should be used instead
      of this.
