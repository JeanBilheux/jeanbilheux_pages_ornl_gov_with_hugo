+++
# Date this page was created.
date = "2018-01-09"

# Project title.
title = "Bragg Edge User Interface (iBeatles)"

# Project summary to display on homepage.
summary = "GUI to automatically fit Bragg Edges, calculate and display strain mapping factors."

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = "project/ibeatles/screen1.png"

# Tags: can be used for filtering projects.
tags = ["python", "pyqt", "braggedge", "pyqtgraph"]

# Optional external URL for project (replaces project detail page).
#external_link = "https://github.com/ornlneutronimaging/iBeatles"

# Does the project detail page use math formatting?
math = false

# Optional featured image (relative to `static/img/` folder).
[header]
image = "project/ibeatles/header.png"
caption = "iBeatles"
+++

Several wavelength-dependent proof-of-concept imaging measurements have been performed at the Spallation Neutron
Source (SNS) VULCAN and SNAP beam lines. This effort is focused on prototyping imaging software for the future SNS
VENUS imaging beam line. One of the time- of-flight (TOF) techniques that is of interest to the scientific community
is the 2-dimensional mapping of phases and average crystalline plane orientation in samples ex-situ, but also during
applied stresses such as tensile loading and heating. This technique is known as Bragg edge imaging and relies on the
identification of changes of transmission values, fitting of the edge to measure its displacement, and thus identify
the shift in lattice parameter due to stresses.

We are implementing these Bragg edge tools using Python in an effort to provide near real-time feedback during an
experiment. A Python library was written to quickly calculate the values of Bragg edges (in the strain-free material)
 for a series of selected crystalline materials. Data in a form of a series of wavelength-dependent radiographs,
 obtained with an micro-channel-plate (MCP) detector, can be quickly loaded using this PyQt User Interface (UI).

Data can be normalized, region of sample can be selected and bin size defined. Then the signal for each particular
bin are automatically fitted. Individual fitting parameters can be displayed as well as the strain mapping of the sample.

For more infos, please check the [Characterization of Crystallographic Structure Using Bragg Edge Neutron Imaging at the Spallation
Neutron Source, Journal of Imaging, 2017, 3, 65](http://www.mdpi.com/2313-433X/3/4/65/htm)

<img src='/img/project/ibeatles/screen1.png' />
During the first step, you load your data and open beams (OB). Thanks to iBeatles, you can compare live your Bragg
edges with the theoretical values of an element of interest.
<hr>

<img src='/img/project/ibeatles/screen2.png' />
You can also display the Bragg edges of one or more region of interest. Moving the region will update live the Bragg edges
peaks.
<hr>

<img src='/img/project/ibeatles/screen3.png' />
The data displayed on the right will come from the single, or multiple files, selected on the left side of the interface.
<hr>

<img src='/img/project/ibeatles/screen4.png' />
The Time Spectra file loaded during the first step can be checked.
<hr>

<img src='/img/project/ibeatles/screen5.png' />
It's possible to select the range of files to display by playing with the list on the left side of the interface, or
by doing a direct selection on the bottom right plot.
<hr>

<img src='/img/project/ibeatles/screen6.png' />
The fitting step allows you to lock any pixel to prevent the program from changing the previously calculated fitting
parameters.
<hr>

<img src='/img/project/ibeatles/screen7.png' />
All the fitting parameters values are displayed using a color scale background as well. This feature allows the user to
quickly determine if a parameter value is way off, which mean that it's certainly wrong.
<hr>

<img src='/img/project/ibeatles/screen8.png' />
It's possible to define an history of the fitting to perform.
<hr>

<img src='/img/project/ibeatles/screen9.png' />
Sometimes, a pre-manipulation of the data is required, such as rotation of the sample. This can be done directly from
the interface itself.
<hr>

<img src='/img/project/ibeatles/screen10.png' />
In this step, the user defines the range of pixels to fit, and the size of each binning block.
<hr>

<img src='/img/project/ibeatles/screen11.png' />
Screenshot showing the ROI tool that allows to select more than one region in order to display and compare the Bragg
edges from several part of the same sample for example.
