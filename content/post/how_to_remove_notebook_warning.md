+++
date = 2018-01-08
draft = false
tags = ["notebook", "python", "widget"]
title = "How to turn off warnings in the notebooks"
summary = """How to handle notebook warnings"""

[header]
image = "posts/how_to_turn_off_notebook_warning/screen1.png"
caption = "Warnings should not be seen by users"
preview = false
+++

If you are annoyed by warning messages (see top picture), Just add the following command at the top of your notebook

```python
import warnings
warnings.filterwarnings(‘ignore’)
```

<img src='/img/posts/how_to_turn_off_notebook_warning/screen2.png' />