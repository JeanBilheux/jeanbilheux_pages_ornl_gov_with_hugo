+++
date = 2018-05-23
draft = false
tags = ["python", "class", "property"]
title = "How python Class properties work"
summary = """Brief review of getter, setter for python classes"""

+++

Using this [tutorial](https://pybit.es/property-decorator.html) I came up with this post that summarize what I need
to remember about **python classes properties**.

Class Properties are very useful in the way that they can check automatically the format of a parameter passed in can
customize, or calculate on the fly, a parameter requested.

Here I define a first parent class


```python
class FlyingHours:

    def __init__(self, owner, starting_hours=0):
        self.owner = owner.title()  # first character is upper case
        self.starting_hours = starting_hours
        self._total_hours = starting_hours

    def __str__(self):
        s = ["{} of {}".format(__class__.__name__, self.owner),
             "Total Hours: {}".format(self._total_hours),
            ]
        return '\n\n'.join(s)
```

```
> o_plane = FlyingHours("jean bilheux", "bob")
> print(o_plane)
FlyingHours of Jean Bilheux

Total Hours: bob
```

Then in a child class, I introduce the getter and setter of the **starting hours** parameter

```python
class FlyingHours1(FlyingHours):

    def __init__(self, owner, starting_hours=0):
        super().__init__(owner, starting_hours=starting_hours)

    @property
    def starting_hours(self):
        return self._starting_hours

    @starting_hours.setter
    def starting_hours(self, hours):
        if not isinstance(hours, int):
            raise TypeError("must be a int")
        if hours <= 0:
            raise ValueError("must be greater than 0!")
        self._starting_hours = hours

    @starting_hours.deleter
    def starting_hours(self):
        raise AttributeError("Cannot reset hours!")

    @property
    def hours(self):
        return "You have been flying {}".format(self._starting_hours)

    def _add_hours(self, hours):
        self._total_hours += hours

    def __iadd__(self, hours):
        'Magic method to allow for acc += amount'
        self._add_hours(hours)
        return self  # need to return object!

    def __isub__(self, hours):
        'Magic method to allow for acc -= amount'
        self._add_hours(-hours)
        return self
```

With this code, the **starting hours** are automatically checked when passed in (even during initialization)

```
> o_plane = FlyingHours1("jean bilheux", starting_hours="zero hours")
> print(o_plane)
...
<ipython-input-3-d84930155a49> in starting_hours(self, hours)
     11     def starting_hours(self, hours):
     12         if not isinstance(hours, int):
---> 13             raise TypeError("must be a int")
     14         if hours <= 0:
     15             raise ValueError("must be greater than 0!")

TypeError: must be a int
```

Then we can retrieve **starting hours** using the getter

```python
> o_plane.starting_hours
10
```

and you can also define your own output using your customize getter
```python
> o_plane.hours
'You have been flying 10'
```

Other cool features (not related to properties) but to **refactoring and computation**

```python
> o_plane += 10
> print(o_plane)
FlyingHours of Jean Bilheux

Total Hours: 20

> o_plane += 20
> print(o_plane)
FlyingHours of Jean Bilheux

Total Hours: 35
```

Notebook tutorial can be found in [python_101](https://github.com/JeanBilheux/python_101/blob/master/python_thinking/properties_class.ipynb)