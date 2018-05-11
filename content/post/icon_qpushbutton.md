+++
date = 2018-05-11
draft = false
tags = ["python", "pyqt", "QPushButton", "GUI"]
title = "How to customize QPushButton Icons"
summary = """QPushButton Icons"""

+++

For one of my project I had to display a customized push button. I found a couple of ways to do this.

The goal is to display the following icon inside a QPushButton.

<img src='/img/posts/icon_qpushbutton/button_rotation_left.png' />

## Using QtGui.QIcon

Let's pretend our image is at */Users/j35/Desktop/button_rotation_left.png*

```python
_icon = QtGui.QIcon("/Users/j35/Desktop/button_rotation_left.png")
my_button = QtGui.QPushButton()
my_button.setIcon(_icon)
my_button.setIconSize(QtCore.QSize(50, 300))
```

<img src='/img/posts/icon_qpushbutton/button_method1.png' />

The problem of using this way, is that the button looses its **push button** feature that changes the look of the
button when the user click it.

## Using StyleSheet

```python
my_button = QtGui.QPushButton()
my_button.setStyleSheet("background-image: url('/Users/j35/Desktop/button_rotation_left.png'); background-repeat: no-repeat")
```

<img src='/img/posts/icon_qpushbutton/button_method2.png' />

### Source

 * [stackOverflow](https://stackoverflow.com/questions/36714078/pyqt4-how-to-make-icon-bigger-than-qpushbutton-pixmap-buttons)
 * [QIcon documentation](http://pyqt.sourceforge.net/Docs/PyQt4/qicon.html)
 * [StyleSheet examples with QPushButton](http://doc.qt.io/archives/qt-4.8/stylesheet-examples.html)
