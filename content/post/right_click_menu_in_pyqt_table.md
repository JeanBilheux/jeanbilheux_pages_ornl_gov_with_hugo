+++
date = 2018-11-01
draft = false
tags = ["python", "pyqt", "QMenu", "widget"]
title = "Right Click Menu in QTable"
summary = """How to bring to life a right click menu in a QTable in PyQt"""

+++

How to get a **right click menu** from a QTable in PyQt

<img src='/img/posts/pyqt_qmenu/final_menu.png' />

To be able to have a right click menu in a QTable (PyQt), you need to follow the following few steps

### Turn on Custom Context Menu

In QtDesigner, and in the **property editor** select in the **contextMenuPolicy** the option **CustomContextMenu**.

<img src='/img/posts/pyqt_qmenu/qtdesigner_setup_1.png' />
<img src='/img/posts/pyqt_qmenu/qtdesigner_setup_2.png' />

### Connect Custom Context Menu Signal

<img src='/img/posts/pyqt_qmenu/signal_editor.png' />

### Catch Signal in Main Code

```python
...
    def h3_table_right_click(self, position):
         o_h3_table = H3TableHandler(parent=self)
         o_h3_table.right_click()

...

class H3TableHandler:

    def __init__(self, parent=None):
        self.parent = parent

    def right_click(self):
        #bar = self.parent.menuBar()
        top_menu = QMenu(self.parent)

        menu = top_menu.addMenu("Menu")
        config = menu.addMenu("Configuration ...")

        _load = config.addAction("&Load ...")
        _save = config.addAction("&Save ...")

        config.addSeparator()

        config1 = config.addAction("Config1")
        config2 = config.addAction("Config2")
        config3 = config.addAction("Config3")

        action = menu.exec_(QtGui.QCursor.pos())

        if action == _load:
            # do this
            pass
        elif action == _save:
            # do this
            pass
        elif action == config1:
            # do this
            pass
        elif action == config2:
            # do this
            pass
        elif action == config3:
            # do this
            pass
```
