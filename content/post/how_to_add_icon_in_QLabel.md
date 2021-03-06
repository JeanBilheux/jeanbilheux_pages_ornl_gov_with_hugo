+++
date = 2018-11-21
draft = false
tags = ["python", "pyqt", "widgets"]
title = "Add an icon inside a QLabel using Qt resource"
summary = """How to take advantage of Qt resources to add an icon inside a label"""
+++

There are many ways to add images or icons on top of buttons or label. Usually the most painful part is
 locating the file in the program and managing the location of those images. Qt comes with a **resource** tool 
 that facilitates this storage. The way I'm doing it may look strange (and redundant sometimes) 
 but at least it works for me so I will stick to it, until I see a better way.
 
## Design your icon
 
 Because I'm lazy, usually I find them on google images and simply resize them. It is important to 
 use a png (not jpeg) as png as a transparent background layer (jpeg does not). For this example I decided
 to work with the following image found on the web.
 
 <img src='/img/posts/how_to_add_icon_in_qlabel/search_icon_original.png' />

This icon is obviously too big. I use **photoshop** to resize it. 

 1. open image in photoshop
 2. click **image/image size
 3. specify pixels as units
 4. enter new width (height will follow). You typically want something around 20 pixels wide. 
 5. save new image
 
 <img src='/img/posts/how_to_add_icon_in_qlabel/how_to_resize_image.gif' />
 
## Bring icon in application

The following step should be possible directly from QtDesigner (using **resource browser) but found out that
I always end up having to do it manually anyway. 

### Copy icon in **icons** folder

If you do not have this folder, create it at the root of your project. This folder should contain a 
**icons.qrc** file that looks like this

```python
<RCC>
    <qresource prefix="MPL Toolbar">
        <file>search_icon.png</file>
        <file>clear_icon.png</file>
        <file>splitter_icon.png</file>
        <file>go-home.png</file>
        <file>zoom-previous.png</file>
        ...
    </qresource>
</RCC>    
```

- You need to edit manually this file and add the new icon (<file>search_icon.png</file> in this case).
- then copy your icon in this folder

### Building the icons_rc.py file

This file is necessary to be able to use the icons in the program. Simply type the following command

```python
pyrcc4 ./icon.qrc -o addie/icons_rc.py
```

*addie* being the subfolder containing all the python code (name of the project).

### Load the image

We should now be able to use the image in our code. 

I usually load all my images in a **init_widgets.py** method called just after bringing the ui to the screen.
If we consider a label called **label_1**, then here is the call to bring the image to the screen

```python
...
self.ui.label_1.setPixmap(QtGui.QPixmap(":/MPL Toolbar/search_icon.png"))
...
```

{{% alert note %}}
in case you are working with a QPushButton instead of a QLabel, here is the way to do the same thing

```python
self.ui.button_1.setIcon(QtGui.QIcon(":/MPL Toolbar/search_icon.png"))
```
{{% /alert %}}

and here is what the application will look like !

<img src='/img/posts/how_to_add_icon_in_qlabel/application_preview.png' />

