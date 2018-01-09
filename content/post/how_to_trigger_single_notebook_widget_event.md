+++
date = 2018-01-08
draft = false
tags = ["notebook", "python", "widget"]
title = "How to trigger only 1 notebook widget dropdown event"
summary = """How to handle notebook widget event"""
+++

I’m working on a notebook that is supposed to clean images. Nothing fancy here, just replacing the negative, **inf** 
and **NaN** values by either **0** or **NaN**. 

This can be done interactively using the following widgets. 

<img src='/img/posts/how_to_trigger_single_notebook_widget_event/screenshot_1.png' />

But my problem is that each time a selection inside one of the dropdown widgets is changed, it triggers 5 events. 
So I added the following snipet of code to cheat the system and only work with the first event triggered.

```python
list_files = _o_fix.list_files
data = _o_fix.data

counter = 0

def give_statistics(index):
 
 _file = os.path.basename(list_files[index])
 _data = data[index]

def on_change(change):
 global counter
 if counter == 0:
 _new_index = change['new']['index']
 _list = change['owner'].options
 print("new value selected is {}".format(_list[_new_index]))
 counter += 1
 elif counter == 4:
 counter = 0
 else:
 counter += 1
 
 stat = get_statistics_of_roi_cleaned(_data)
 
 number_of_pixels = stat.total_pixels
 negative_values = stat.nbr_negative
 negative_percentage = stat.percentage_negative
 infinite_values = stat.nbr_infinite
 infinite_percentage = stat.percentage_infinite
 nan_values = stat.nbr_nan
 nan_percentage = stat.percentage_nan
 
 box1 = widgets.HBox([widgets.Label("File Name:"),
 widgets.Label(_file,
 layout=widgets.Layout(width='80%'))])
 box2 = widgets.HBox([widgets.Label("Total number of pixels:",
 layout=widgets.Layout(width='15%')),
 widgets.Label(str(number_of_pixels))])
 
 box3 = widgets.HBox([widgets.Label("Negative values:",
 layout=widgets.Layout(width='10%')),
 widgets.Label("{} pixels ({:.3}%)".format(negative_values, negative_percentage),
 layout=widgets.Layout(width='15%')),
 widgets.Label("to replace by"),
 widgets.Dropdown(options=['NaN', '0'],
 value='NaN')])
 neg_widgets = box3.children[3]
 neg_widgets.observe(on_change)
 
 box4 = widgets.HBox([widgets.Label("Infinite values:",
 layout=widgets.Layout(width='10%')),
 widgets.Label("{} pixels ({:.3}%)".format(infinite_values, infinite_percentage),
 layout=widgets.Layout(width='15%')),
 widgets.Label("to replace by"),
 widgets.Dropdown(options=['NaN', '0'],
 value='NaN')])

box5 = widgets.HBox([widgets.Label("NaN values:",
 layout=widgets.Layout(width='10%')),
 widgets.Label("{} pixels ({:.3}%)".format(nan_values, nan_percentage),
 layout=widgets.Layout(width='15%')),
 widgets.Label("to replace by"),
 widgets.Dropdown(options=['NaN', '0'],
 value='NaN')])


 vertical_box = widgets.VBox([box1, box2, box3, box4, box5])
 display(vertical_box)
 
 _result_inf = np.where(np.isinf(_data))
 _data[_result_inf] = np.NaN
 
 fig, [[ax0, ax1], [ax2, ax3]] = plt.subplots(ncols=2, nrows=2,
 figsize=(15, 10),
 num=os.path.basename(_file))

# plt.title(os.path.basename(_files[index]))
 cax0 = ax0.imshow(_data, cmap='viridis', interpolation=None)
 ax0.set_title("Raw Image")
 tmp1 = fig.colorbar(cax0, ax=ax0) # colorbar

ax1.hist(_data.ravel(), range=(np.nanmin(_data), np.nanmax(_data)), bins=256)
 ax1.set_title("Raw Histogram")

cax2 = ax2.imshow(_data, cmap='viridis', interpolation=None)
 ax2.set_title("New Image")
 tmp1 = fig.colorbar(cax2, ax=ax2) # colorbar

ax3.hist(_data.ravel(), range=(np.nanmin(_data), np.nanmax(_data)), bins=256)
 ax3.set_title("New Histogram")

fig.tight_layout()
 
 
tmp3 = widgets.interact(give_statistics,
 index=widgets.IntSlider(min=0,
 max=len(list_files) - 1,
 step=1,
 value=0,
 description='File Index',
 continuous_update=False),
 )

```

The code here shows how to connect the dropdown widget to an event and how to only handle the first event triggered.

