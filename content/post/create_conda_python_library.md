+++
date = 2018-03-09
draft = false
tags = ["python", "conda"]
title = "How to create a python library using conda"
summary = """Create conda python library"""

+++

The goal of this post will be show you the minimum requirements to build a conda library.

The starting project will be a library I have been working on called [NeuNorm](https://github.com/scikit-beam/NeuNorm).

First thing, create a folder **conda-recipes** that will contain the files necessary to build the library.

In this folder, I created 2 files

**conda_build_config.yaml**
```
python:
  - 2.7
  - 3.5
  - 3.6
```

and

**meta.yaml**
```
{% set version = "1.3.16" %}
{% set git_rev = "1.3.16" %}

package:
  name: neunorm
  version: {{ version }}

source:
  git_rev: {{ git_rev }}
  git_url: https://github.com/scikit-beam/NeuNorm

build:
  number: 0
  script: python setup.py install --single-version-externally-managed --record record.txt
#  noarch: python

requirements:
  build:
    - python {{ python }}
    - setuptools
  run:
    - python
    - pillow
    - numpy
    - scipy
    - pathlib
    - astropy

test:
  imports:
    - NeuNorm

about:
  home: https://github.com/scikit-beam/NeuNorm
  license: BSD 3-Clause
  summary: Neutron imaging normalization tool
```

For each new release, make sure you change the top version number.

Now, let's build it by first going into that folder **conda-recipes**

> cd conda-recipes

then

> conda build .

{{% alert note %}}
In order to upload your library to conda, you need to create an account in [Anaconda cloud](https://anaconda.org/conda-forge).
In my case, I also created a **organization** called **neutronimaging**. I will use that *channel* to upload my library.
<img src='/img/posts/create_conda_python_library/anaconda_cloud_group.png' />
{{% /alert %}}

You simply need to follow the direction given by anaconda
> anaconda upload /users/j35/anaconda/conda-bld/osx-64/neunorm-1.3.16-py* -u neutronimaging

Voila !

You will find your library in the anaconda cloud web site

<img src='/img/posts/create_conda_python_library/neunorm_library.png' />

The instructions here are for the simplest case. For more complicated build, check [the anaconda documentation](https://conda.io/docs/user-guide/tutorials/build-pkgs.html)
or the following tutorial by [Gavin Wiggins](https://github.com/wigging/python-package).

Big thanks to Jiao Lin for showing me how to do it, step by step.



