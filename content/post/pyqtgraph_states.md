+++
date = 2018-03-05
draft = false
tags = ["notebook", "python", "pyqtgraph"]
title = "Keep Pan and Zoom of ImageView When Displaying New Image"
summary = """Extra work needs to be done to make sure the pan and zoom done in the ImageView is preseverd when
displaying a new image"""
preview = false
+++

The ImageViewer of pyqtgraph is amazing but comes with an annoying feature, the reset of the **pan** and **zoom**
if you display a new image (as shown here)

<img src='/img/posts/pyqtgraph_states/before_saving_states.gif' />

A couple of lines of codes need to be added in order to preserve those settings when switching image. Looks like
nothing but it took me a couple of hours to figure it out so here it is with code before and after

### Definition of ImageView

```python
    def init_pyqtgraph(self):
        ...
        area = DockArea()
        area.setVisible(True)
        d1 = Dock("Sample Image", size=(200, 300))
        d2 = Dock("Profile", size=(200, 100))
        d3 = Dock("Water Intake", size=(200, 400))

        area.addDock(d1, 'top')
        area.addDock(d2, 'bottom')
        area.addDock(d3, 'right')

        # image view
        self.ui.image_view = pg.ImageView(view=pg.PlotItem())

        self.ui.image_view.ui.menuBtn.hide()
        self.ui.image_view.ui.roiBtn.hide()
        ...
```

then the part about the display of the images

### Before

```python

    def refresh_image(self):
        first_update = False
        if self.histogram_level == []:
            first_update = True
        _histo_widget = self.ui.image_view.getHistogramWidget()
        self.histogram_level = _histo_widget.getLevels()

        index_selected = self.ui.file_index_slider.value()
        _image = self.dict_data['list_data'][index_selected-1]
        _image = np.transpose(_image)
        _image = self._clean_image(_image)
        _image = self._force_range(_image)
        self.current_image = _image
        self.ui.image_view.setImage(_image)
```

### After

```python
    def refresh_image(self):

        _view = self.ui.image_view.getView()
        _view_box = _view.getViewBox()
        _state = _view_box.getState()

        first_update = False
        if self.histogram_level == []:
            first_update = True
        _histo_widget = self.ui.image_view.getHistogramWidget()
        self.histogram_level = _histo_widget.getLevels()

        index_selected = self.ui.file_index_slider.value()
        _image = self.dict_data['list_data'][index_selected-1]
        _image = np.transpose(_image)
        _image = self._clean_image(_image)
        _image = self._force_range(_image)
        self.current_image = _image
        self.ui.image_view.setImage(_image)
        _view_box.setState(_state)
```

This allows to get the following behavior

<img src='/img/posts/pyqtgraph_states/pyqtgraph_save_state.gif' />

