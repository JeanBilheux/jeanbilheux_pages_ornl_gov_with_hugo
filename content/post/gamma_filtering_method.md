+++
date = 2018-03-09
draft = false
tags = ["python", "neunorm", "imaging", "normalization"]
title = "Gamma Filtering"
summary = """How do we remove the abnormal high counts (gammas) from our images"""
preview = false
+++

# Introduction

When working with neutorn imaging files, one of the first thing we need to do is removing the gamma spots. Those
spots are pixel that recorded gammas strikes, not neutrons strikes.

To do so, we are going to use the **convolve** library from **scipy**.

# Initialize Fake Data Set

```python
import numpy as np
from scipy.ndimage import convolve

# let's create a 10x10 random array of values between 0 and 1
my_array = np.random.rand(10, 10)

# let's fake a couple of gammas at [5, 5] and [7, 7]
my_array[5, 5] = 10
my_array[7, 7] = 9
```

<img src='/img/posts/gamma_filtering/before_cleaning.png' />

# Cleaning

The principle is that we will look for all the pixels for which a fraction of their value (coefficient that can
 be set) is still above the mean of the entire image. We will then replace those high counts pixels by the average value
of all the pixels surrounding that pixel.

```python
# threshold coefficient
coefficient = 0.1

mean_value = np.mean(my_array)
indexes = np.where( coefficient * my_array > mean_value)

mean_kernel = np.array([[1, 1, 1], [1, 0, 1], [1, 1, 1]]) / 8.0
convolved_data = convolve(my_array, mean_kernel, mode='constant')

cleaned_array = my_array.copy()
cleaned_array[indexes] = convolved_data[indexes]
```

<img src='/img/posts/gamma_filtering/after_cleaning.png' />


