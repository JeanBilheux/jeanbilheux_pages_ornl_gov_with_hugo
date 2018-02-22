+++
date = 2018-02-20
draft = false
tags = ["numpy", "int"]
title = "How to handle a list of int and improve its display"
summary = """Improve list of ints display"""
+++

# Introduction

In one of my project, I had to handle a list of runs and improve the way it was displayed.

For example


```
10,20,21,22,23,24,30,40,41,15,18
```

should be displayed


```
10,15,18,20:24,30,40,41
```

Also, if a user add new runs to the list, using a dropdown list on the side, the list will be updaded as followed:

 * if a number is already in the list, it will be removed
 * if a number is new to the list, it will just be added

Before showing you the code, here is how it works using a few examples

# Examples

### example 1

```python
from __code.utilities import ListRunsParser

o_parser = ListRunsParser(current_runs="1,2,4:15")
o_parser.list_current_runs
new_runs = ['6','8', '10']
o_parser.new_runs(list_runs=new_runs)
```

the output will be
```
>> 1,2,4,5,7,9,11:15
```

and if we provide new runs again

```python
new_runs = ['6','8', '10']
o_parser.new_runs(list_runs=new_runs)
```

```
>> 1,2,4:15
```

### example 2

```python
o_parser = ListRunsParser(current_runs="1,2,3,10")
o_parser.list_current_runs
new_runs = ['6']
o_parser.new_runs(list_runs=new_runs)
```

```
>> 1:3,6,10
```

### example 3

```python
o_parser = ListRunsParser(current_runs="")
o_parser.list_current_runs
new_runs = ['6','8','10','11','12']
o_parser.new_runs(list_runs=new_runs)
```

```
>> 6,8,10:12
```

# Implementation

```python

class ListRunsParser(object):
    """
    will clean up the current_list_of_runs with the new added runs
    ex: [1,2,3,4,7] -> 1:4,7
    if a new run is already in the list of runs, it will then be removed from the list
    ex: [1,2,3,4] with new run [1] -> 2:4
    """

    list_current_runs = []  # ['1','10','2','30','4']
    int_list_current_runs = []  # [1, 2, 4, 10, 30]

    def __init__(self, current_runs=''):
        if current_runs:
            self.make_discrete_list_of_runs(str_current_runs=current_runs)

    def make_discrete_list_of_runs(self, str_current_runs=''):
        spans = (el.partition(':')[::2] for el in str_current_runs.split(','))
        ranges = (np.arange(int(s), int(e) + 1 if e else int(s) + 1)
                  for s, e in spans)
        all_nums = itertools.chain.from_iterable(ranges)
        _all_nums = set(all_nums)
        self.list_current_runs = [str(_run) for _run in _all_nums]

    def new_runs(self, list_runs=[]):
        """add new runs, remove already existing ones"""

        # find list of runs to remove
        list_runs = set(list_runs)
        _list_runs_to_remove = set(list_runs.intersection(self.list_current_runs))

        # remove the runs from list_runs and list_current_runs
        clean_list_runs = list(list_runs - _list_runs_to_remove)
        clean_list_current_runs = list(set(self.list_current_runs) - \
                                       _list_runs_to_remove)

        new_list_current_runs = clean_list_runs + clean_list_current_runs
        self.list_current_runs = new_list_current_runs

        # go from string to int
        int_new_list_current_runs = [np.int(_run) for _run in new_list_current_runs]

        # sort them to prepare them for output format
        int_new_list_current_runs.sort()
        self.int_list_current_runs = int_new_list_current_runs

        if int_new_list_current_runs == []:
            self.str_list_current_runs = ""
            return

        # create output string format

        # only 1 run
        if len(int_new_list_current_runs) == 1:
            self.str_list_current_runs = str(int_new_list_current_runs[0])
            return

        # more than 1 run

        # create full matching list
        def match_list(reference_list=[], our_list=[]):
            _index = 0
            _ref_list_and_our_list = zip(our_list, reference_list)
            for _ref_run, _our_run in _ref_list_and_our_list:
                if _ref_run == _our_run:
                    _index += 1
                    continue
                break

            return _index

        _index = 0
        _groups = []
        _our_list = self.int_list_current_runs[_index: ]
        _list_full_reference = np.arange(_our_list[0], _our_list[-1]+1)

        print("new list: {}".format(_our_list))

        while _our_list:

            _ref_index = match_list(reference_list=_list_full_reference,
                                    our_list=_our_list)

            _group = [_our_list[0], _our_list[_ref_index-1]]
            # print("_group: {}".format(_group))
            _groups.append(_group)

            _our_list = _our_list[_ref_index:]
            if len(_our_list) == 1:
                _groups.append(_our_list)
                break

            if len(_our_list) == 0:
                break

            _list_full_reference = np.arange(_our_list[0], _our_list[-1]+1)

        print("_groups: {}".format(_groups))

        list_runs = []
        for _group in _groups:

            if len(_group) == 2:
                [_left_value, _right_value] = _group

                if _left_value == _right_value:
                    list_runs.append(str(_left_value))
                elif _right_value == (_left_value + 1):
                    list_runs.append(str(_left_value))
                    list_runs.append(str(_right_value))
                else:
                    list_runs.append("{}:{}".format(_left_value, _right_value))

            else:
                list_runs.append(str(_group[0]))

        str_runs = ",".join(list_runs)
        print(str_runs)

```

And here is the implementation of this tool in a notebook.

<img src='/img/posts/list_of_int_parser/live_demo.gif' />