+++
date = 2018-11-06
draft = false
tags = ["python", "pyqt", "QTableWidget", "QTree"]
title = "Multi rows table (QTableWidget)and Qtree interacting together"
summary = """Tree and Table interacting together"""
+++

For one of my projects, I needed to answer the following requirements:

 * table with 3 rows of labels (each row of label is a submenu of the row above)
 * resizing any of the column will resize accordingly the other rows
 * scroll of any of the label rows will scroll the other labels
 * tree on the size will allow to hide/show any of the column
 * hidding any of the tree should automatically hide the leaves under it
 * if hidding all the leaves of a branch, automatically hide the branch parent
 * user should have the option to save and reload a configuration
 * full reset of table is available
 * user can remove configurations he created
 
*Because QTableWidget does not allow several rows of label, I had to built this feature from scratch.*

 This project is available on our python notebooks repository (https://github.com/neutronimaging/python_notebooks). 
 
 Once installed (with dependencies), start the **addie.py** notebook and run the cells. 
 
 Here is the program in action
 
<img src='/img/posts/addie_table/addie_table_prototype.gif' />
