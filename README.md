# μsort

**Safe, minimal import sorting for Python projects.**

[![version](https://img.shields.io/pypi/v/usort.svg)](https://pypi.org/project/usort)
[![changelog](https://img.shields.io/badge/change-log-blue.svg)](https://github.com/facebookexperimental/usort/blob/main/CHANGELOG.md)
[![license](https://img.shields.io/pypi/l/usort.svg)](https://github.com/facebookexperimental/usort/blob/main/LICENSE)
[![code style](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

μsort is a safe, minimal import sorter. Its primary goal is to make no "dangerous"
changes to code, and to make no changes on code style. This is achieved by detecting
distinct "blocks" of imports that are the most likely to be safely interchangeable, and
only reordering imports within these blocks without altering formatting. Code style
is left as an exercise for linters and formatters.

Within a block, µsort will follow common Python conventions for grouping imports based
on source (standard library, third-party, first-party, or relative), and then sorting
lexicographically within each group. This will commonly look like:

```py
import re
from pathlib import Path
from typing import Iterable
from unittest.mock import expectedFailure, Mock, patch

import aiohttp
from aiosqlite import connect

import foo
from bar import bar

from .main import main
```

Blocks are inferred from a number of real world conditions, including any intermediate
statements between imports:

```py
import warnings
warnings.filterwarnings(...)

import re
import sys
```

In this case, µsort detects two blocks–separated by the call to `filterwarnings()`,
and will only sort imports inside of each block. Running µsort on this code
will generate no changes, because each block is already sorted.

Imports can be excluded from blocks using the `# usort:skip` directive, or with
`# isort:skip` for compatibility with existing codebases. µsort will leave
these imports unchanged, and treat them as block separators.

See the [User Guide][] for more details about how blocks are detected,
and how sorting is performed.


## Install

µsort requires Python 3.6 or newer to run. Install µsort with:

```shell-session
$ pip install usort
```


## Usage

To format one or more files or directories in-place:

```shell-session
$ usort format <path> [<path> ...]
```

To generate a diff of changes without modifying files:

```shell-session
$ usort diff <path>
```

To just validate that files are formatted correctly, like during CI:

```shell-session
$ usort check <path>
```

# License

μsort is MIT licensed, as found in the [LICENSE][] file.

[LICENSE]: https://github.com/facebookexperimental/usort/tree/main/LICENSE
[User Guide]: https://usort.readthedocs.io/
