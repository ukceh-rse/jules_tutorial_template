---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.16.7
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

# Experiment Title

```python
from pathlib import Path

import pandas as pd
import matplotlib.pyplot as plt
import xarray as xr
```

## Construct paths

Construct some `pathlib.Path` objects to make path building simpler and more robust.

```python
# This should say /path/to/jules_tutorial_template
! git rev-parse --show-toplevel
```

```python
(git_root,) = ! git rev-parse --show-toplevel
git_root = Path(git_root)
devbox_json = git_root / "portable-jules" / "devbox.json"

assert devbox_json.exists()
```

```python
# This should say /path/to/jules_tutorial_template/experiment !
! pwd
```

```python
# Construct some path objects to make path building simpler and more robust
notebook_dir = Path.cwd()

jules_dir = notebook_dir / "jules"
namelists_dir = jules_dir / "config"
inputs_dir = jules_dir / "inputs"
outputs_dir = jules_dir / "outputs"

assert jules_dir.exists()
```

## Run JULES

```python
! devbox run -c {devbox_json} jules -d {jules_dir} {namelists_dir}
```

```python
! cat -n {jules_dir / "stdout.log"} | (head; tail)
```

```python
! cat {jules_dir / "stderr.log"}
```

```python
! ls {outputs_dir}
```

## Inspect the output data

```python
ds = xr.open_dataset(outputs_dir / "loobos.test.nc")

ds
```

### Inspect tstar

```python
tstar = ds.tstar.squeeze()

tstar
```

```python
tstar.plot.line(x = "time")
```

```python
tstar_df = tstar.to_dataframe().drop(["latitude", "longitude"], axis=1)
tile_ids = tstar_df.index.get_level_values("tile").unique()

tstar_rolling = pd.concat(
    {f"tile {tile_id}": tstar_df.xs(tile_id, level = "tile").tstar.rolling(window="24h").mean() for tile_id in tile_ids},
    axis=1,
)

tstar_rolling
```

```python
tstar_rolling.plot(ylabel="K", label="Daily average temp")
```

```python

```
