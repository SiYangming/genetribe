# Conda recipe (YangmingSi channel)

Build and upload (already published as `yangmingsi::genetribe=1.2.1`):

```bash
conda build .
anaconda upload -u YangmingSi noarch/genetribe-1.2.1-py_0.conda
```

Install:

```bash
conda install -c yangmingsi -c bioconda -c conda-forge genetribe=1.2.1
```

Package page: https://anaconda.org/YangmingSi/genetribe
