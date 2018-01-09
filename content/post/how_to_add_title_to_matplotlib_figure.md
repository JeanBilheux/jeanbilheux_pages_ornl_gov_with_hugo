+++
date = 2018-01-08
draft = false
tags = ["notebook", "python", "widget", "matplotlib"]
title = "How to add a title to a matplotlib figure inside a notebook"
summary = """How to handle matplotlib figure title"""
+++

By default, matplotlib only provides an increment index to the figure title as shown here

```python
_file_name = _files[index]
fig, (ax0, ax1) = plt.subplots(ncols=2, figsize=(10,5))

#plt.title(os.path.basename(_files[index]))
cax0 = ax0.imshow(_data[index], cmap='viridis', interpolation=None)
ax0.set_title("Before Correction")
_ = fig.colorbar(cax0, ax=ax0) # colorbar

cax1 = ax1.imshow(_clean_data[index], cmap='viridis', interpolation=None)
ax1.set_title("After Correction")
_ = fig.colorbar(cax1, ax=ax1) # colorbar

fig.tight_layout()
```

<img src='/img/posts/how_to_add_title_to_matplolib_figure/screen1.png' />

In order to provide a title to this plot, simply add a value to the flag **num** in **plt.subplots**

```python
_file_name = _files[index]
fig, (ax0, ax1) = plt.subplots(ncols=2, figsize=(10,5),
                               num=os.path.basename(_file_name))

#plt.title(os.path.basename(_files[index]))
cax0 = ax0.imshow(_data[index], cmap='viridis', interpolation=None)
ax0.set_title("Before Correction")
_ = fig.colorbar(cax0, ax=ax0) # colorbar

cax1 = ax1.imshow(_clean_data[index], cmap='viridis', interpolation=None)
ax1.set_title("After Correction")
_ = fig.colorbar(cax1, ax=ax1) # colorbar

fig.tight_layout()
```

<img src='/img/posts/how_to_add_title_to_matplolib_figure/screen2.png' />
