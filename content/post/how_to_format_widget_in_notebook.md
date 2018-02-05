+++
date = 2018-02-05
draft = false
tags = ["notebook", "python", "widget"]
title = "How to format the notebook widgets"
summary = """Use CSS to format widgets inside a notebook"""
+++

When working with widgets inside a notebook, the default style looks like this

<img src='/img/posts/how_to_format_widget_in_notebook/default_style.png' />

In order to change its style, it's possible to modify the CSS style sheet as followed

```
from IPython.core.display import HTML
HTML("""
<style>
.mytext > .widget-label {
    font-style: strong;
    color: black;
    font-size: 30px;
}
.mytext > input[type="text"] {
    font-size: 20px;
    color: green;
}
</style>
""")
```

And this will give a new style as shown here

<img src='/img/posts/how_to_format_widget_in_notebook/customized_style.png' />

{{% alert note %}}
Thanks to Kynan for [the following post](https://stackoverflow.com/questions/32156248/how-do-i-set-custom-css-for-my-ipython-ihaskell-jupyter-notebook)
 who helped me figuring out the solution
{{% /alert %}}
