+++
date = 2018-03-02
draft = false
tags = ["notebook", "python", "pyqtgraph"]
title = "Working with the histogram widget of pyqtgraph"
summary = """Avoid reset of pyqtgraph histogram"""
preview = false
+++

The histogram tool on the side of the image coming for free with pyqtgraph is amazing. You can change or move the
range but any refresh of the image reset the settings defined. Here is how you can fix those settings.

<img src='/img/posts/pyqtgraph_histogram/histogram_showing_issue.gif' />

My main challenge has been to recover the **id** of the histogram when not initializing this one myself. And here is
the way

```python

class MyPyqt(QMainWindow):

    histogram_level = []

    def __init__(self):
        self.init_pyqtgraph()

    def init_pyqtgraph(self):

        # image view
        self.ui.image_view = pg.ImageView()
        self.ui.image_view.ui.menuBtn.hide()
        self.ui.image_view.ui.roiBtn.hide()

        ...


    def refresh_image(self):

        first_update = False
        if self.histogram_level == []:
            first_update = True

        _histo_widget = self.ui.image_view.getHistogramWidget()
        self.histogram_level = _histo_widget.getLevels()

        # retrieve image data to display
        index_selected = self.ui.file_index_slider.value()
        _image = self.dict_data['list_data'][index_selected-1]
        _image = np.transpose(_image)
        _image = self._clean_image(_image)
        _image = self._force_range(_image)
        self.current_image = _image
        self.ui.image_view.setImage(_image)

        if not first_update:
            _histo_widget.setLevels(self.histogram_level[0], self.histogram_level[1])
```

<img src='/img/posts/pyqtgraph_histogram/histogram_fixed.gif' />

You can find the full documentation of the histogram feature by going to the pyqtgraph documentation web page

[<img src='/img/posts/pyqtgraph_histogram/histo_doc.png' />](http://www.pyqtgraph.org/documentation/graphicsItems/histogramlutitem.html)