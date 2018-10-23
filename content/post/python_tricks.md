+++
date = 2018-10-23
draft = false
tags = ["python"]
title = "Python Tricks"
summary = """Various little tricks to simplify your life using Python"""
+++

## Locate absolute home folder on all OS

```python
from os.path import expanduser
print(expanduser("~"))
```

**On Windows**

```python
'C:\\Users\\j35`
```

**On Mac**

```python
'/Users/j35'
```