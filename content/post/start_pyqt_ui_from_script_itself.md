+++
date = 2018-10-16
draft = false
tags = ["python", "pyqt"]
title = "Launching PyQt from script"
summary = """What you need to know to start a simple pyqt ui from a script file"""
+++

I designed the very simple following UI using QtDesigner and called it **main_window.ui**. I placed it in the 
**ui_template** folder. 

<img src='/img/posts/start_pyqt_ui_from_script_itself/main_ui.png' />

Here is what the structure of my project looks like.

<img src='/img/posts/start_pyqt_ui_from_script_itself/tree_structure.png' />

I have my own script that buitl automatically the python version of the template

**setup.py**
```python
import os


class UiBuilder(object):
    pyuic = ''

    def __init__(self, ui_name=''):
        self.define_pyuic_to_run()
        self.ui_name = os.path.abspath('ui_template/' + ui_name)
        [base, ext] = os.path.splitext(ui_name)
        py_name = base + '.py'
        self.py_name = os.path.abspath('ui/' + py_name)

        # run command
        print(self.pyuic + ' ' + self.ui_name + ' -o ' + self.py_name)
        os.system(self.pyuic + ' ' + self.ui_name + ' -o ' + self.py_name)

    def define_pyuic_to_run(self):
        try:
            from PyQt4 import QtGui
            self.pyuic = 'pyuic4'
        except:
            from PyQt5 import QtGui
            self.pyuic = 'pyuic5'


if __name__ == "__main__":
    o_builder = UiBuilder(ui_name='main_window.ui')
```

to run it, simply do

```
> python setup.py
```

We can now import and use the interface in our main script, called here **main.py**

```python
import sys

try:
    from PyQt4.QtGui import QFileDialog
    from PyQt4 import QtCore, QtGui
    from PyQt4.QtGui import QMainWindow
except ImportError:
    from PyQt5.QtWidgets import QFileDialog
    from PyQt5 import QtCore, QtGui
    from PyQt5.QtWidgets import QApplication, QMainWindow

from ui.main_window  import Ui_MainWindow as UiMainWindow


class Interface(QMainWindow):

    def __init__(self, parent=None):

        QMainWindow.__init__(self, parent=parent)
        self.ui = UiMainWindow()
        self.ui.setupUi(self)
        self.setWindowTitle("Anton's MCP Detector Efficiency Correction UI")


if __name__ == "__main__":
    app = QApplication(sys.argv)
    o_interface = Interface()
    o_interface.show()
    sys.exit(app.exec_())
```

to start the application

```
> python main.py
```

