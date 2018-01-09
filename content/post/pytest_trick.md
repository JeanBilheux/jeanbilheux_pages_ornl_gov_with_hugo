+++
date = 2018-01-06
draft = false
tags = ["python", "pytest"]
title = "pytest trick to run script before and after each test"
summary = """pre and post unit test script"""
+++

I recently had to write a set of unit test that required some code to be 
executed before* and after each test. I found  a few post that listed the wrong method name 
so here is how I did it. 

```python

import unittest

def setUp(self): 
   _file_path = os.path.dirname(__file__)
   self.data_path = os.path.abspath(os.path.join(_file_path, '../data/'))
   self.export_folder = self.data_path + '/temporary_folder/'
   os.mkdir(self.export_folder)
 
def tearDown(self):
   shutil.rmtree(self.export_folder)
 
def test_export_create_the_right_file_name(self):
'''assert export works for all data types for tif output'''

  sample_path = self.data_path + '/tif/sample'
  ob_path = self.data_path + '/tif/ob'
  o_norm = Normalization()
  o_norm.load(folder=sample_path)
  o_norm.load(folder=ob_path, data_type='ob')
  
  # OB
  o_norm.export(folder=self.export_folder, data_type='ob') 
  _output_file_name_0 = o_norm._export_file_name[0]
 
  _file_name_0 = os.path.basename(o_norm.data['ob']['file_name'][0])
  _new_file_name = os.path.splitext(_file_name_0)[0] + '.tif'  
  _expected_file_name_0 = os.path.join(self.export_folder,   _new_file_name)
 
  self.assertTrue(_expected_file_name_0 == _output_file_name_0)
 
  # Normalized
  o_norm.normalization()
  o_norm.export(folder=self.export_folder, data_type='normalized')
  _output_file_name_0 = o_norm._export_file_name[0]
 
  _file_name_0 = os.path.basename(o_norm.data['sample']['file_name'][0])
  _new_file_name = 'normalized_' + os.path.splitext(_file_name_0)[0] + '.tif'
  _expected_file_name_0 = os.path.join(self.export_folder, _new_file_name)
  self.assertTrue(_expected_file_name_0 == _output_file_name_0) 
 

```

The magic takes place in the **setUp** and **tearDown** methods.