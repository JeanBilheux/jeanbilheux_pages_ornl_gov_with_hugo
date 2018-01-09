+++
date = 2018-01-09
draft = false
tags = ["python", "notebook", "matplotlib", "jupyter"]
title = "Create a Target type tool in matplotlib"
summary = """matplotlib target tool"""
+++

In one of my project, I had to select, for various images, the best center of a circular object. I came up with the
following solution that, when used with the jupyter notebooks widgets, helps finding the best center of the object.

Here is such an image where we need to find the center. In this case, it's pretty obvious, but in real cases it's much
more complicated so this tool should really help.

<img src='/img/posts/target_tool_in_matplotlib/screen1.png' />

Here is the selection tool in action.

<img src='/img/posts/target_tool_in_matplotlib/screen2.png' />

and here is the code used to generate this tool

```python
from matplotlib import collections as mc
import matplotlib.pyplot as plt
%matplotlib notebook

rom ipywidgets.widgets import interact
from ipywidgets import widgets

#working_data is a numpy array of your data
...

[height, width] = np.shape(working_data)

def select_center(x0, y0):

 fig, ax = plt.subplots()
 ax.imshow(working_data)

 min_mark_size = 10 #pixels

 #show center
 plt.axvline(x=x0)
 plt.axhline(y=y0)

 #show symetrical mark on (x0,y0) reference lines to help figure
 # out the right center
 nbr_x_ref_lines = 5
 nbr_y_ref_lines = 5

 # calculate the min distance between center and edges
 working_width = np.min([x0, width-x0])
 working_height = np.min([y0, height-y0])

 if working_width <= 4 * nbr_x_ref_lines:
   nbr_x_ref_lines = 1
 elif working_height <= 4 * nbr_y_ref_lines:
   nbr_y_ref_lines = 1

 #determine the reference lines coordinates
 x_interval = working_width / (nbr_x_ref_lines + 1)
 y_interval = working_height / (nbr_y_ref_lines + 1)

 references_lines = []

 #right of x0
 for i in np.arange(nbr_x_ref_lines):
   mark_size = (i+1) * min_mark_size
   point1 = (x0 + (i+1) * x_interval, y0 - mark_size)
   point2 = (x0 + (i+1) * x_interval, y0 + mark_size)
   references_lines.append([point1, point2])

 #left of x0
 for i in np.arange(nbr_x_ref_lines):
   mark_size = (i+1) * min_mark_size
   point1 = (x0 - (i+1) * x_interval, y0 - mark_size)
   point2 = (x0 - (i+1) * x_interval, y0 + mark_size)
   references_lines.append([point1, point2])

 #top of y0
 for j in np.arange(nbr_y_ref_lines):
 mark_size = (j+1) * min_mark_size
 point1 = (x0 - mark_size, y0 - (j+1) * y_interval)
 point2 = (x0 + mark_size, y0 - (j+1) * y_interval)
 references_lines.append([point1, point2])

 #bottom of y0
 for j in np.arange(nbr_y_ref_lines):
   mark_size = (j+1) * min_mark_size
   point1 = (x0 - mark_size, y0 + (j+1) * y_interval)
   point2 = (x0 + mark_size, y0 + (j+1) * y_interval)
   references_lines.append([point1, point2])

 #calculate list of colors
 basic_color = (1,0,0,1)
 list_color = [basic_color for x in np.arange(len(references_lines))]

 lc = mc.LineCollection(references_lines, colors=list_color, \
                        linewidths=2)
 ax.add_collection(lc)


preview = interact(select_center,
   x0 = widgets.IntSlider(min=0,
   max=width-1,
   value=np.int(width/2),
   description = 'x0'),
   y0 = widgets.IntSlider(min=0,
   max=height-1,
   value=np.int(height/2),
   descrption= 'y0'))
```

