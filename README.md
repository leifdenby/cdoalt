# cdoalt - an alternative cdo interface for python

This repository contains a proof-of-concept interface for using
[cdo](https://code.mpimet.mpg.de/projects/cdo/) from python.

Every call to cdo in `cdoalt` is made of three steps:

1. call `cdoalt.cdo(...)` with the parameters you need for the call to `cdo`. These might for example be the output-filetype (`fout_filetype`) or whether to run with verbose output (`verbose=True`)

2. do any number of cdo operations, for example `.sellonlatbox(...)`, `.remap(...)` passing parameters for each and chaining one onto the next.

3. `.execute(...)` the cdo call, providing the input and output filenames

A full call to cdo might look like the following:

```python
# coding: utf-8
from pathlib import Path
from cdoalt import cdo

GRID_FILES_PATH = Path("/work/ka1081/DYAMOND/PostProc/GridsAndWeights")

res_deg = 0.1

grid_filename = GRID_FILES_PATH / f"{res_deg:.02f}_grid.nc"
weights_filename = GRID_FILES_PATH / f"ICON_R2B09_2_{res_deg:.02f}_grid_wghts.nc"
in_filename = "MPI-M/ICON-5km/DW-CPL/atmos/15min/rlut/dpp0029/ml/gn/rlut_15min_ICON-5km_DW-CPL_dpp0029_ml_gn_20200120000000-20200120234500.nc"

(
    cdo(verbose=True)
    .selname("rlut")
    .remap(grid_filename, weights_filename)
    .sellonlatbox(-70, -60, 0, 10)
    .execute(in_filename, "out.nc")
)
```

## Why `cdoalt`?

- `cdoalt` allows for function chaining as is common when using other python packages (for example [numpy](https://numpy.org/) and [xarray](xarray.pydata.org/)

- `cdoalt` gives doc-strings which describe what each operation (and the parameters of the operation) do

## Further ideas / thoughts

- add parsing of cdo operators as [Cdo.py](https://github.com/Try2Code/cdo-bindings)

    - include docstring for each operator including arguments

    - add individual operators using a decorator to register operators

    - cache operators using `~/.cache` directory


# Credit

Another very nice cdo-python library is available at https://github.com/Try2Code/cdo-bindings
