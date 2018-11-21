+++
date = 2018-01-09
draft = true
tags = ["notebook", "python", "html", "jupyter", "sphinx"]
title = "Automatically convert notebooks into html page for documentation using sphinx"
summary = """Convert notebooks into html"""
+++

I'm going to show you here how you can easily create an HTML document from a notebook.

1 - First thing you need is make sure your notebook has a title

```python
# this is my title
```

Then, make sure that all the other headings of the notebooks have a lower hierarchy.

<img src='/img/posts/convert_notebooks_to_html_pages/screen1.png' />

2 - Then you will need to import the following python libraries

```
> pip install sphinx
> pip install nbconvert
> pip install pandoc
> pip install latex
> pip install nbsphinx
```

{{% alert note %}}
Somehow, I had to use conda for the **pandoc** library to work.
{{% /alert %}}


```
> conda install pandoc
```

3 -  Move your notebooks into the source folder, next to the *index.rst* file for example

4 -  Modify the *index.rst* by adding the name of the notebooks to display

<img src='/img/posts/convert_notebooks_to_html_pages/screen2.png' />

{{% alert note %}}
This example is based on the [NeuNorm](https://github.com/ornlneutronimaging/NeuNorm) project.
{{% /alert %}}

5 - Let's now modify the **conf.py** file

```
extensions = ['sphinx.ext.autodoc',
              'nbsphinx',
              'sphinx.ext.mathjax',
              'sphinx.ext.githubpages',
              'IPython.sphinxext.ipython_console_highlighting

...

source_suffix = ['.rst', '.ipynb']
```

6 - Everything we did so far now allows us to insert the notebook inside the documentation, but only locally. To
make it work via *ReadTheDocs*, we need the next step describes here.

Create a **readthedocs.yml** file into our source tree

```
# REad the Docs config

# python version
python:
  version: 3.5
  pip_install: True

# build a PDF
formats:
  - none

# use a conda environment file
conda:
  file: documentation/readthedocs-environment.yml
```

And then, add another file called **readthedocs-environments.yml** inside our documentation folder

```
name: NeuNorm
channels:
  - conda-forge
dependencies:
  - python==3.5
  - pandoc
  - nbformat
  - jupyter_client
  - ipython
  - nbconvert
  - sphinx>=1.5.1
  - ipykernel
  - sphinx_rtd_theme
  - pip:
    - nbsphinx
```

7 - Let's change the *Admin* settings in ReadTheDocs as followed

 1. Go to **Advanced Settings** of **Admin** menu.

 2. Specify the name of the python configuration file

    ```
    documentation/source/conf.py
    ```

 3. and also the python interpreter
    ```
    CPython 3.x
    ```

8 - Now, let's build the documentation

```
> python3 -m sphinx shource build
> make html
```

and here is the local documentation produced

<img src='/img/posts/convert_notebooks_to_html_pages/screen3.png' />

and here is the ReadTheDocs documentation created

<img src='/img/posts/convert_notebooks_to_html_pages/screen4.png' />




