+++
date = 2018-01-09
draft = false
tags = ["python", "pyqt", "ui"]
title = "Getting started with PyQt applications"
summary = """PyQt application 1.0.1"""

[header]
image = "posts/pyqt_applications/screen1.png"
caption = "What we try to achieve in this tutorial"
+++

Getting started with a PyQt application is alwawys intimidating. We always have a few important questions when starting such a
project:

* what tools do I need? PyQt, python, Qt Designer...etc.
* how should I structure my project
* how to compile it?

Here is a step by step instructions that shows you how to get started. I'm not telling you this is the right way (if there
is any) or even the best way to do it, but I know this way works for me, so here it is.

## Step 1 - Requirements

For this tutorial, you will need the following

* [Qt Designer](https://download.qt.io/official_releases/qt/4.8/4.8.7/). I'm using version 4.8.7 in this tutorial
* python 3.5 (I recommend the use of [anaconda](https://www.anaconda.com/download/#macos) to install python and create
your own environment.
* pyqt. I'm using version 4.11.4 that I installed this way

```
> conda install -c anaconda pyqt=4*
```

## Step 2 - Clone the Base Empty Project

To facilitate the work, I created a template repository that contains the base files you need to start such an application.
You will find this repository here ([PyQtGuiBase](https://github.com/JeanBilheux/PyQtGuiBase))

#### What you will find in this repo

- **designer**: You will "draw" your Graphical User Interface (GUI) in the **designer** folder.
- **mygui**, named after your project name, contains all the python code that brings to life all your widgets in your
application.
- **scripts** is the point of entry of the application

Other files are all typical of any python project, so nothing new here. Please refer to the python documentation if you
want to know more about those files.

## Step 3 - Design the Interface

First, let's review what we are trying to achieve here.

<img src='/img/posts/pyqt_applications/screen1.png' />

 A simple window with a button that allows the loading of an image. A text field that shows the path to the file you
selected and then a window that displays the image loaded.

But before we can start enjoying this application, we need to design it.

Start **QtDesigner**, select **Main Window** in the default welcome window, or click **File > New ...**

<img src='/img/posts/pyqt_applications/screen2.png' />

Click **Create**.

You are now facing a blank canvas

<img src='/img/posts/pyqt_applications/screen3.png' />

On the left side, you should see a list of widgets available in Qt.

<img src='/img/posts/pyqt_applications/screen4.png' />

*Drag and drop* a **Push Button** and a **Line Edit** into the canvas. Make sure the button is on the left side of the line
edit widget as shown here.

<img src='/img/posts/pyqt_applications/screen5.png' />

To nicely align those widgets, we gonna define an horizontal layout around those first widgets. Select the **button** and the
**line edit** and then *Right Click*, select *Lay out > Lay Out Horizontally*.

<img src='/img/posts/pyqt_applications/screen6.png' />

In order to display the image loaded, we will need to have a widget that we will later "upgrade" into a matplotlib class.
*Drag and Drop* a simple **widget** into the canvas, just below our first 2 widgets you just aligned.

<img src='/img/posts/pyqt_applications/screen7.png' />

*Right click* the background of the Main Window and select a vertical layout.

<img src='/img/posts/pyqt_applications/screen8.png' />
<img src='/img/posts/pyqt_applications/screen9.png' />

The top widgets are taking way too much space compare to the widget (future plot) at the bottom. Select the bottom
widget and in the **property editor**, change the *vertical policy* to *Expanding*

<img src='/img/posts/pyqt_applications/screen10.png' />

and voila!

<img src='/img/posts/pyqt_applications/screen11.png' />

## Step 4 - Connecting the widgets to the python code

Right now the widgets can not do anyting as they are not connected to any event handler. Let's now do that.

But first, let's rename our widgets to something that has more sense that the **pushButton** and **lineEdit**.

You can eithe click every widget and the use the **Property Editor** to change their name, or you can use the **Object Inspector**.

<img src='/img/posts/pyqt_applications/screen12.png' />

*Right Click* on the **LineEdit** and **PushButton** objects and select *Change objectName*.

<img src='/img/posts/pyqt_applications/screen13.png' />

Give the following new names to our widgets

* pushButton -> browse_button
* lineEdit -> tiff_file_name

<img src='/img/posts/pyqt_applications/screen14.png' />

Now, let's connect the button to a signal. We want to trigger a python function when the user clicks the button.

There are several ways to do this. Let's pick the easiest way for this tutorial. *Click* the **signal editor** button from the
**widget box window**.

<img src='/img/posts/pyqt_applications/screen15.png' />

This allows you to select how the widget will behave. *Click and hold* the **browse_button** and *release* the mouse when
you see the ground symbol.

<img src='/img/posts/pyqt_applications/screen16.png' />

Now, in the new window that just poped up, select **clicked()** and then **click** *edit...* to define the name of your
python function that will handle the click.

<img src='/img/posts/pyqt_applications/screen17.png' />
<img src='/img/posts/pyqt_applications/screen18.png' />

call this function *browse_button_clicked()*

Then **click OK** and, once you are back in the previous window, select the function you just created and click **OK**.

<img src='/img/posts/pyqt_applications/screen19.png' />

Congratulations, your button is alive. Does not do anything yet, but alive!

Before leaving QtDesigner for good, one last cosmetic thing. Let's change the label of the button to inform the user of
the meaning of this button.

Make sure you switch back to **Edit Widgets** mode

<img src='/img/posts/pyqt_applications/screen20.png' />

Then double click the **Push button** and type "*Browse...*"

Last thing. Save your UI!

## Step 5 - Compile our UI

Right now, if you take a look at the ui file you just saved via QtDesigner, it's an XML that python can not use. Let's fix that.

Switching to a command line console (such as **terminal** or **iTerm** on Mac). The **setup.py** file you found with this
project allows you to simply pass a *pyuic* argument to make the compilation happenging.

```
python setup.py pyuic
```

Because you are using the setup.py file I provide with the project, the command is very short. In reality it's a little bit
more complex than that and looks like

```
pyuic4 ui/my_ui.ui -o designer/my_ui.py
```

## Step 6 - Python Code at Last

Using your favorite editor (emacs, vi, pyCharm, wingIDE....), let's add the code that will bring our interface to life

*scripts/myGUI*
```python
#!/usr/bin/env python

#myGui

import sys
import os

import PyQt4
import PyQt4.QtCore as QtCore
import PyQt4.QtGui as QtGui
from PyQt4.QtGui import QMainWindow as QMainWindow
from PyQt4.QtGui import QApplication

import mygui.interfaces.ui_mainWindow



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


    def browse_button_clicked(self):
        pass

def main():
    app = QApplication(sys.argv)
    application = MainWindow()
    application.show()
    app.exec_()

if __name__ == '__main__':
    main()
```

Make sure you add your project to your python path

example here for Mac
```
export PYTHONPATH=<full_path_to_root_of_your_project>;$PYTHONPATH
```

## Step 7 - Testing your Application for the First Time

You can now test the application by running the main script

```
> python scripts/myGUI
```

<img src='/img/posts/pyqt_applications/screen21.png' />

Nothing exciting so far, but it's alive!

## Step 8 - Connecting the Widgets

Because I like to keep the main script as lean as possible, we are going to take care of loading the files in another
class, let's call it **FileHandler** in the *file_handler.py* file.

Create a file *file_handler.py* inside the **mygui** folder and add the following code

```python
#file_handler.py
from PyQt4.QtGui import QFileDialog
from PIL import Image


class FileHandler(object):

    data = []

    def __init__(self, parent=None):
        self.parent = parent

    def browse_tiff_file(self):


        file_name = str(QFileDialog.getOpenFileName(caption = "Select the TIFF file",
                                                filter = "tiff (*.tif)"))

        if file_name:
            self.parent.ui.tiff_file_label.setText(file_name)
            self.load_image(file_name)
            self.display_image()

```

Then in order to use this class in the main script, import the class by adding the following line at the top of the **myGUI** file

```python
#myScript
from mygui.file_handler import FileHandler

...
```

To simplify everyting, we are going to use the same *file_handler.py* file to take care of loading the image. Update
the *file_handler.py* file by adding the following lines

```python
#file_handler.py

from PyQt4.QtGui import QFileDialog
from PIL import Image


class FileHandler(object):

    data = []

    def __init__(self, parent=None):
        self.parent = parent

    def browse_tiff_file(self):


        file_name = str(QFileDialog.getOpenFileName(caption = "Select the TIFF file",
                                                filter = "tiff (*.tif)"))

        if file_name:
            self.parent.ui.tiff_file_label.setText(file_name)

```

## Step 9 - Plotting the Image

Final step is to display our image using matplotlib.

First thing, let's connect the *browse button* to this script

```python
#!/usr/bin/env python

#myGui

import sys
import os

import PyQt4
import PyQt4.QtCore as QtCore
import PyQt4.QtGui as QtGui
from PyQt4.QtGui import QMainWindow as QMainWindow
from PyQt4.QtGui import QApplication

import mygui.interfaces.ui_mainWindow

from mygui.file_handler import FileHandler


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


    def browse_button_clicked(self):
        o_file_handler = FileHandler(parent = self)
        o_file_handler.browse_tiff_file()


def main():
    app = QApplication(sys.argv)
    #app.setWindowIcon(PyQt4.QtGui.QIcon(":/icon.png"))
    application = MainWindow()
    application.show()
    app.exec_()

if __name__ == '__main__':
    main()
```

And then, let's bring the matplotlib to life by adding the following lines to the main script

```python
#!/usr/bin/env python

#myGui

import sys
import os

import PyQt4
import PyQt4.QtCore as QtCore
import PyQt4.QtGui as QtGui
from PyQt4.QtGui import QMainWindow as QMainWindow
from PyQt4.QtGui import QApplication

from matplotlib.backends.backend_qt4agg import FigureCanvasQTAgg as FigureCanvas
from matplotlib.backends.backend_qt4agg import NavigationToolbar2QT as NavigationToolbar
from matplotlib.figure import Figure

import mygui.interfaces.ui_mainWindow
from mygui.file_handler import FileHandler


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

        # init matplotlib
        self.figure = Figure()
        self.axes = self.figure.add_subplot(111)
        self.canvas = FigureCanvas(self.figure)
        self.toolbar = NavigationTollbar(self.canvas, self)

        layout = QtGui.QVBoxLayout()
        layout.addWidget(self.canvas)
        layout.addWidget(self.toolbar)
        self.ui.widget.setLayout(layout)

    def browse_button_clicked(self):
        o_file_handler = FileHandler(parent = self)
        o_file_handler.browse_tiff_file()


def main():
    app = QApplication(sys.argv)
    application = MainWindow()
    application.show()
    app.exec_()

if __name__ == '__main__':
    main()
```

If you run this program at this point, you should see an empty axis

<img src='/img/posts/pyqt_applications/screen22.png' />

The only thing left to do is to collect the data (array coming from the image) and show it in the canvas.

```python
#file_handler.py

from PyQt4.QtGui import QFileDialog
from PIL import Image


class FileHandler(object):

    data = []

    def __init__(self, parent=None):
        self.parent = parent

    def browse_tiff_file(self):


        file_name = str(QFileDialog.getOpenFileName(caption = "Select the TIFF file",
                                                filter = "tiff (*.tif)"))

        if file_name:
            self.parent.ui.tiff_file_label.setText(file_name)
            self.load_image(file_name)
            self.display_image()

    def load_image(self, filename):
        data = Image.open(filename)
        self.data = data

    def display_image(self):
        self.parent.ui.widget.canvas.ax.imshow(self.data)
        self.parent.ui.widget.draw()
```

<img src='/img/posts/pyqt_applications/screen1.png' />
