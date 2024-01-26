<!-- These are examples of badges you might want to add to your README:
     please update the URLs accordingly

[![Built Status](https://api.cirrus-ci.com/github/<USER>/dolomite-mae.svg?branch=main)](https://cirrus-ci.com/github/<USER>/dolomite-mae)
[![ReadTheDocs](https://readthedocs.org/projects/dolomite-mae/badge/?version=latest)](https://dolomite-mae.readthedocs.io/en/stable/)
[![Coveralls](https://img.shields.io/coveralls/github/<USER>/dolomite-mae/main.svg)](https://coveralls.io/r/<USER>/dolomite-mae)
[![PyPI-Server](https://img.shields.io/pypi/v/dolomite-mae.svg)](https://pypi.org/project/dolomite-mae/)
[![Conda-Forge](https://img.shields.io/conda/vn/conda-forge/dolomite-mae.svg)](https://anaconda.org/conda-forge/dolomite-mae)
[![Monthly Downloads](https://pepy.tech/badge/dolomite-mae/month)](https://pepy.tech/project/dolomite-mae)
[![Twitter](https://img.shields.io/twitter/url/http/shields.io.svg?style=social&label=Twitter)](https://twitter.com/dolomite-mae)
-->

[![Project generated with PyScaffold](https://img.shields.io/badge/-PyScaffold-005CA0?logo=pyscaffold)](https://pyscaffold.org/)
[![PyPI-Server](https://img.shields.io/pypi/v/dolomite-se.svg)](https://pypi.org/project/dolomite-se/)
![Unit tests](https://github.com/ArtifactDB/dolomite-se/actions/workflows/pypi-test.yml/badge.svg)

# Save and load `MultiAssayExperiments` in Python

## Introduction

The **dolomite-mae** package is the Python counterpart to the [**alabaster.mae**](https://github.com/ArtifactDB/alabaster.mae) R package,
providing methods for saving/reading `MultiAssayExperiment` objects within the [**dolomite** framework](https://github.com/ArtifactDB/dolomite-base).
All components of the `MultiAssayExperiment` - column_data, sample map and experiments - are saved to their respective file representations,
which can be loaded in a new R/Python environment for cross-language analyses.

## Quick start

Let's mock up a `MultiAssayExperiment`:

```python
import summarizedexperiment
import biocframe
import numpy

se = summarizedexperiment.SummarizedExperiment(
    assays={ "counts": numpy.random.rand(1000, 200) },
    row_data=biocframe.BiocFrame(
        { "foo": numpy.random.rand(1000) }, 
        row_names = ["gene" + str(i) for i in range(1000)]
    ),
    column_data=biocframe.BiocFrame(
        { "whee": numpy.random.rand(200) },
        row_names = ["cell" + str(i) for i in range(200)]
    )
)
```

Now we can save it:

```python
from dolomite_base import save_object
import dolomite_se
import os
from tempfile import mkdtemp

path = os.path.join(mkdtemp(), "test")
save_object(se, path)
```

And load it again, e,g., in a new session:

```python
from dolomite_base import read_object

roundtrip = read_object(path)
## Class SummarizedExperiment with 1000 features and 200 samples
##   assays: ['counts']
##   row_data: ['foo']
##   column_data: ['whee']
```
