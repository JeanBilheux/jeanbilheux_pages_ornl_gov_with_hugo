+++
date = 2018-01-12
draft = false
tags = ["python", "pyqt"]
title = "How to lock all the signals of a widget"
summary = """Managing signals from any widget"""
+++

I had to work on a project where the modification of any cell of a big table had to trigger an **event**. But there were
cases where I had to repopulate the entire table. The problem of doing so is that each cell will then trigger the **event**.

The solution is very simple. All you need is to **block all the signals** before updating the table (for example) and then
**unlock the signals** once it's done.

```python
try:
    from PyQt4 import QtGui
except ImportError:
    from PyQt5 import QtGui

...

# table_roi is the name of my ui
self.ui.table_roi.blockSignals(True)

# populate table

self.ui.table_roi.blockSignals(False)
```
<img src='/img/posts/pyqt_block_signals/screen1.png' />


