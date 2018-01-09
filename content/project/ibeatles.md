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
<img src='/img/project/ibeatles/screen2.png' />
<img src='/img/project/ibeatles/screen3.png' />
<img src='/img/project/ibeatles/screen4.png' />
<img src='/img/project/ibeatles/screen5.png' />
<img src='/img/project/ibeatles/screen6.png' />
<img src='/img/project/ibeatles/screen7.png' />
<img src='/img/project/ibeatles/screen8.png' />
<img src='/img/project/ibeatles/screen9.png' />
<img src='/img/project/ibeatles/screen10.png' />
<img src='/img/project/ibeatles/screen11.png' />



<html lang="en-us">
<head>
		<script type='text/javascript' src='http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js'></script>
        <script type='text/javascript' src='/unitegallery/js/unitegallery.min.js'></script>
		<link rel='stylesheet' href='/unitegallery/css/unite-gallery.css' type='text/css' />
		<script type='text/javascript' src='/unitegallery/themes/default/ug-theme-default.js'></script>
		<link rel='stylesheet' href='/unitegallery/themes/default/ug-theme-default.css' type='text/css' />

</head>
<body>
<div id="gallery" style="display:none;">

			<img alt="Image 1 Title" src="/img/project/ibeatles/screen1.png"
				data-image="/img/project/ibeatles/screen1.png"
				data-description="Image 1 Description">

			<img alt="Image 2 Title" src="/img/project/ibeatles/screen2.png"
				data-image="/img/project/ibeatles/screen2.png
				data-description="Image 2 Description">

		</div>

<script type="text/javascript">

			jQuery(document).ready(function(){
                jQuery("#gallery").unitegallery({
		    		gallery_theme: "compact"
    			});

			});

		</script>
</body>
</html>