+++
# Date this page was created.
date = "2018-11-21"

# Project title.
title = "QTableWidget (with 3 rows of headers) and Tree interacting together"

# Project summary to display on homepage.
summary = "A 3 header rows QTableWidget working in sync with a tree to hide or not columns"

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = "project/table_and_tree_prototype/prototype.png"

# Tags: can be used for filtering projects.
tags = ["python", "pyqt", "QTableWidget", "QTree"]

# Optional external URL for project (replaces project detail page).
#external_link = "https://github.com/neutrons/addie"

# Does the project detail page use math formatting?
math = false

# Optional featured image (relative to `static/img/` folder).
[header]
image = "project/table_and_tree_prototype/addie_header_video.gif"
caption = "iBeatles"
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
 
 Once installed (with dependencies), start the **test_addie_table.py** notebook and run the cells. 
 
<img src='/img/project/table_and_tree_prototype/addie_notebook_preview.png' />

