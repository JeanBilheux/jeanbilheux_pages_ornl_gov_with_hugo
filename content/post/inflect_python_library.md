+++
date = 2018-10-16
draft = false
tags = ["python", "documentation", "library"]
title = "How to automatically plural words in Python"
summary = """Inflect tutorial to plural words in Python"""
+++

**inflect** correctly generate plurals, singular nouns, ordinals, indefinite articles; convert numbers to words.parameters

## Installation

This library can be installed via **pip**

```
> pip install inflect 
```

and the full documentation of this library can be found [here](https://pypi.org/project/inflect/)

## How Does it Work?

When do we need this tool

```python
print("You want to see {} variable".format(user_variable))
```

No matter the value of user_variable, the output will always be something like

```
You want to see X variable
```

**Problem here!!**

Using now this new library

```python
import inflect
p = inflect.engine()
print("You want to see {}.format(user_variable) + p.plura("variable", user_variable)
```

if user_variable is 1

```
You want to see 1 variable
```

but if user_variable is 3 for exam ple

```
You want to see 3 variables
```

NB: I used this library for the first time in the notebook *metadata_ascii_parser.ipynb*

