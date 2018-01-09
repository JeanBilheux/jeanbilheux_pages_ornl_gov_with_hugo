+++
date = 2018-01-09
draft = false
tags = ["python", "unittest", "pytest"]
title = "Use of local file in unit test"
summary = """unit test"""
+++

I had some issue in one of my project when running my unit test from the command line and from my IDE. I could never
have both happy at the same time. This was related to the way I was calling local file used in my unit tests.

Using the command line, I had to refer to the file using relative path, but the IDE was looking for full path (which
is obviously wrong). Here is the trick to make both of them happy at the same time.

Here is how my *tests* folder is structured

<img src='/img/posts/call_local_file_in_unit_test/screen1.png' />

And here is where the magic happens. This is done in the **setUp** method at the beginning of every unit test class that
will need to use those local files.

*experiment_test.py*
```python
class ExperimentTest(unittest.TestCase):

    def setUp(self):
        _file_path = os.path.dirname(__file__)
        self.data_path = os.path.abspath(os.path.join(_file_path, '../../data'))

...
```

and here, this is how you use it

```python
def test_experiment_calculate_lambda(self):
    _tof_file = os.path.join(self.data_path, 'tof.txt')
    _tof_obj = TOF(filename = _tof_file)
    ...
```