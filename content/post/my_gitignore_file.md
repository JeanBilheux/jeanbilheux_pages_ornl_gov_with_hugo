+++
date = 2018-01-08
draft = false
tags = ["github", "python", "notebook", "jupyter"]
title = "What my .gitignore file looks like"
summary = """List of files to ignore in github"""
+++

When working with notebooks and python projects, here is what my **.gitignore** file looks like.

```
__pycache__

*.py[cod]

.ipynb*
.coverage
/cover/*

# mac
.DS_Store

.cache

# Packages
*.egg
*.egg-info
dist
build
eggs
parts
bin
var
sdist
MANIFEST

# Unit test / coverage reports
.coverage
```
