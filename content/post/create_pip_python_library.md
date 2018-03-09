+++
date = 2018-01-09
draft = false
tags = ["python", "pip"]
title = "How to create a python library using pip"
summary = """Create pip python library"""

+++

I have been working on a small project that allows to calculate the radial profile of an image for a given angular range.
We are not interested in this post about the functionality of the library itself, but in case you are curious, here
is where you can find the source code [SectorizedRadialProfile](https://github.com/JeanBilheux/SectorizedRadialProfile).

I, first, ned to mention that the code is fully documented using [sphinx](https://pypi.python.org/pypi/Sphinx) and fully
unit tested using [pytest](https://pypi.python.org/pypi/pytest).

I learned to create packages using the well documented [python packaging user guide[(https://packaging.python.org/tutorials/distributing-packages/#requirements-for-packaging-and-distributing),
but I still decided to make this short tutorial in order to quickly remind me the key points.

## Requirements

First you need to install **twine**

```
> pip install twine
```

## configuring the project

**setup.py**

This is a very important file. This is where you define the metadata of your project. For this project, here is my
*setup.py* file

```python
#!/usr/bin/env python
from setuptools import setup, find_packages

setup(
    name = "sectorizedradialprofile",
    version = "1.0.0",
    author = "Jean Bilheux",
    author_email = "bilheuxjm@ornl.gov",
    packages = find_packages(exclude=['tests', 'notebooks']),
    include_package_data = True,
    test_suite = 'tests',
    install_requires = [
        'numpy',
        'pyfits',
        'pillow',
    ],
    dependency_links = [
    ],
    description = "Radial profile of a given sector of an 2D array",
    license = 'BSD',
    keywords = "tiff tif profile radial sector",
    url = "https://github.com/JeanBilheux/SectorizedRadialProfile",
    classifiers = ['Development Status :: 3 - Alpha',
                   'Topic :: Scientific/Engineering :: Physics',
                   'Intended Audience :: Developers',
                   'Programming Language :: Python :: 2.7',
                   'Programming Language :: Python :: 3.5'],
)

# End of file
```


**setup.cfg**

This file contains the default option for *setup.py*

```python
[bdist_wheel]
universal=1

[bdist]
formats=rpm
```

**README.rst**

This is usually where the user will go first to learn how to use your library, so there is important.

<img src='/img/posts/create_python_library/screen1.png' />

**MANIFEST.in**

This is where you define the library needed by your package. In this project, I only needed *numpy* but I still left
this empty, leaving it up to the user to install it on its own.

## Packaging the project

```
> python setup.py sdist
> python setup.py bdist_wheel --universal
```

## Upload the distribution

1. First, we needt to get a [PyPi user account](https://pypi.python.org/pypi?%3Aaction=register_form).

2. We can then upload the distribution

```
> twine upload dist/*
```

<img src='/img/posts/create_python_library/screen2.png' />

and voila !

Your package is now available on PyPi and can be installed using pip

```
> pip install sectorizedradialprofile
```

<img src='/img/posts/create_python_library/screen3.png' />

## Sum up

To sumarize, and using a tag version this time

```
> git tag X.Y.Z -m "Relase X.Y.Z."
> git push --tags
```

and then

```
> pip install --upgrade twine wheel
> python setup.py sdist bdist_wheel --universal
> twine upload dist/*
```

(Thank you to [Dan Bader](https://dbader.org/) for this final shortcut tip!)

