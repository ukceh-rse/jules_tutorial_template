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

<!-- #raw -->
! devbox run -c ../portable-jules/devbox.json jules -d jules/ jules/config
<!-- #endraw -->

```python
from pathlib import Path

import matplotlib.pyplot as plt
import xarray as xr
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

## Inspect the output data

```python
dataset = xr.open_dataset(outputs_dir / "loobos.test.nc")

dataset
```

```python
print("hi")
```
