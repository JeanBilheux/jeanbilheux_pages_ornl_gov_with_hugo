+++
date = 2019-03-04
draft = false
tags = ["python", "pyqt", "GUI", "QTDesigner", "ui"]
title = "How to load ui at run time in PyQt application."
summary = """Run time loading of ui files created with QTDesigner."""

+++

In this post, I will show how you can load at run time a couple of QWidget created in QTDesigner. Each of those
QWidget will be placed in their own tab. 

The final application will look like this

<img src='/img/posts/load_ui_at_run_time_pyqt/tab1_main_application.png' />

## QTDesigner work

Using QTDesigner, I created:
 
  * 1 **QWidget ui** named *tab1.ui*
  * 1 **QWidget ui** named *tab2.ui*
  * 1 **QMainWindow ui** named *ui_mainWindow.ui*

<img src='/img/posts/load_ui_at_run_time_pyqt/designer_tab1.png' />
<center>tab1.ui</center>

<img src='/img/posts/load_ui_at_run_time_pyqt/designer_tab2.png' />
<center>tab2.ui</center>

<img src='/img/posts/load_ui_at_run_time_pyqt/designer_main_window.png' />
<center>ui_mainWindow.ui</center>
  
Those files are saved in the same folder **designer**

Using pyuic5, I created the .py files in the **interfaces** folder.

```python
> pyuic5 designer/ui_mainWindow.ui -o mygui/interfaces/ui_mainWindow.py
```

Here is how the project is structured

<img src='/img/posts/load_ui_at_run_time_pyqt/file_tree.png' />

## pyCharm work

Using pyCharm, here is how I made the import possible

```python
#!/usr/bin/env python

import sys
from importlib import reload

try:
    import PyQt4
    import PyQt4.QtCore as QtCore
    import PyQt4.QtGui as QtGui
    from PyQt4.QtGui import QMainWindow as QMainWindow
    from PyQt4.QtGui import QApplication 
except:
    import PyQt5
    import PyQt5.QtCore as QtCore
    import PyQt5.QtGui as QtGui
    from PyQt5.uic import loadUi
    from PyQt5 import QtWidgets
    from PyQt5.QtWidgets import QMainWindow as QMainWindow
    from PyQt5.QtWidgets import QApplication, QVBoxLayout, QWidget, QPushButton, \
        QRadioButton, QHBoxLayout

import mygui.interfaces.ui_mainWindow

import mygui.interfaces.tab1
reload(mygui.interfaces.tab1)

import mygui.interfaces.tab2
reload(mygui.interfaces.tab2)


class MainWindow(QMainWindow, mygui.interfaces.ui_mainWindow.Ui_MainWindow):

    def __init__(self):
        """ Initialization
        Parameters
        ----------
        """
        # Base class
        QMainWindow.__init__(self)

        # Initialize the UI widgets
        self.ui = mygui.interfaces.ui_mainWindow.Ui_MainWindow()
        self.ui.setupUi(self)

        # import tab1
        self.tab1_widget = QtWidgets.QWidget()
        ui = mygui.interfaces.tab1.Ui_MyTab1()
        ui.setupUi(self.tab1_widget)
        self.ui.tabWidget.insertTab(0, self.tab1_widget, "tab1")

        # import tab2
        self.tab2_widget = QtWidgets.QWidget()
        ui = mygui.interfaces.tab2.Ui_Form()
        ui.setupUi(self.tab2_widget)
        self.ui.tabWidget.insertTab(1, self.tab2_widget, "tab2")

def main():
    app = QApplication(sys.argv)
    application = MainWindow()
    application.show()
    app.exec_()

if __name__ == '__main__':
    main()

```
### Source

 * [Answer replied in PyQt mailing list](https://stackoverflow.com/questions/36714078/pyqt4-how-to-make-icon-bigger-than-qpushbutton-pixmap-buttons)
