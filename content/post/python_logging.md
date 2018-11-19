+++
date = 2018-11-07
draft = false
tags = ["python", "logging"]
title = "Python logging module 1.0.1"
summary = """How to use the logging module in python"""
+++

This tutorial is a short version of the following [full tutorial](https://realpython.com/python-logging/)

## Quick Demo

First, we need to import the library to be able to use it

```python
import logging
```

Then to use it

```python
import logging

logging.debug('This is a debug message')
logging.info('This is an info message')
logging.warning('This is a warning message')
logging.error('This is an error message')
logging.critical('This is a critical message')

def test_me():
    logging.debug('This is a debug message')
    logging.info('This is an info message')
    logging.warning('This is a warning message')
    logging.error('This is an error message')
    logging.critical('This is a critical message')

if __name__ == "__main__":
    test_me()
```

The output of this program will be

```
WARNING:root:This is a warning message
ERROR:root:This is an error message
CRITICAL:root:This is a critical message
WARNING:root:This is a warning message
ERROR:root:This is an error message
CRITICAL:root:This is a critical message
```

By default, the logging module logs only everything that is WARNING level or higher. But it's possible to change
this by playing with the **basicConfig**

## Basic Configurations

Here are some of the main features of the configurations:

 * **level**: The root logger will be set to the specified severity level.
 * **filename**: This specifies the file.
 * **filemode**: If filename is given, the file is opened in this mode. The default is a, which means append.
 * **format**: This is the format of the log message.
 
 Example:
 
 To now log everything at or above DEBUG
 
 ```python
 import logging

 logging.basicConfig(level=logging.DEBUG) 
 logging.debug("This will get logged now")
 ```
 
 output will be
 
  ```shell
 DEBUG:root:This will get logged now
 ```
 
 ### logging into a file
 
 If we want to log everything in a file instead of just in the terminal
 
```python
import logging

logging.basicConfig(filename='/Users/j35/Desktop/app.log', filemode='w', format='%(name)s - %(levelname)s - %(message)s')
logging.warning("This will get logged to a file")
```

Here is the code I used to test it

```python
import logging

def try_me():
    logging.debug('This is a debug message')
    logging.info('This is an info message')
    logging.warning('This is a warning message')
    logging.error('This is an error message')
    logging.critical('This is a critical message')

if __name__ == "__main__":

    logging.basicConfig(filename='/Users/j35/Desktop/app.cfg',
                        filemode='w',
                        format='%(name)s - %(levelname)s - %(message)s')
    logging.warning("This should go to a file")
    
    try_me()
```

and here is the *app.cfg* file created

```python
root - WARNING - This should go to a file
root - WARNING - This is a warning message
root - ERROR - This is an error message
root - CRITICAL - This is a critical message
```

To get a full list of all the parameters that can be set, go [here](https://docs.python.org/3/library/logging.html#logging.basicConfig)
 
## How to personalize the output

To get a full list of all the parameters we can simply retrieve and display in the message, go [here](https://docs.python.org/3/library/logging.html#logrecord-attributes)

But here is an example of what we can do using those parameters

```python
import logging

logging.basicConfig(format='%(process)d-%(levelname)s-%(message)s')
logging.warning('This is a Warning')
```

this will produce the following output

```python
18472-WARNING-This is a Warning
``` 
 
In this example, we add the date and time

```python
import logging

logging.basicConfig(format='%(asctime)s - %(message)s', level=logging.INFO)
logging.info('Admin logged in')
``` 

```python
2018-07-11 20:12:06,288 - Admin logged in
```

### Logging our own data

```python
import logging
name = 'John'
logging.error('%s raised an error', name)
``` 

```python
ERROR:root:John raised an error
``` 

 or even better using the new python 3.6 format
 
 ```python
import logging
name = 'John'
logging.error(f'{name} raised an error')
```
 
### Capturing Exceptions

It's also possible to capture the entire stack trace by turning on the **exc_info** flag as shown here

```python
import logging

a = 5
b = 0

try:
  c = a / b
except Exception as e:
  logging.error("Exception occurred", exc_info=True)
``` 

```python
ERROR:root:Exception occurred
Traceback (most recent call last):
  File "exceptions.py", line 6, in <module>
    c = a / b
ZeroDivisionError: division by zero
[Finished in 0.2s]
``` 

We can also use this, **if we call it from inside an exception handler**

```python
import logging

a = 5
b = 0
try:
  c = a / b
except Exception as e:
  logging.exception("Exception occurred")
```

and this will produce te same output. 

## Classes and functions

In a real project, we should define our own **logger** by creating an object of the logger class. 

Unlike the root logger, a custom logger will not display by default the name of the logger

```python
import logging
logger = logging.getLogger('example_logger')
logger.warning('This is a warning')
```

which produces the following output

```python
This is a warning
```

WARNING

unlike the root logger, a custom logger can not be configured using **basicConfig()**, instead we need to use **Handlers**
and **Formatters**.

### Using Handlers

Handlers are required when we want to configure our own logger and for example, sent the various messages (error, warnings, ...)
to different outputs (screen, http, file, ...).

Example: Let's create a logger that record everything with level WARNING and above to the screen, but everything with
level ERROR should also be saved to a file

```python
# logging_example.py

import logging

# Create a custom logger
logger = logging.getLogger(__name__)

# Create handlers
c_handler = logging.StreamHandler()
f_handler = logging.FileHandler('file.log')
c_handler.setLevel(logging.WARNING)
f_handler.setLevel(logging.ERROR)

# Create formatters and add it to handlers
c_format = logging.Formatter('%(name)s - %(levelname)s - %(message)s')
f_format = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
c_handler.setFormatter(c_format)
f_handler.setFormatter(f_format)

# Add handlers to the logger
logger.addHandler(c_handler)
logger.addHandler(f_handler)

logger.warning('This is a warning')
logger.error('This is an error')
```

and that produce the following output

```python
__main__ - WARNING - This is a warning
__main__ - ERROR - This is an error
```

and create a file

```python
# file.log
2018-08-03 16:12:21,723 - __main__ - ERROR - This is an error
```

### Other Configuration Methods

It's also possible to configure our logger using a **config file** (or a
dictionary) and loading it using *fileConfig()* (or *dictConfig()*).

Example of a config file called *file.conf*
```python
[loggers]
keys=root,sampleLogger

[handlers]
keys=consoleHandler

[formatters]
keys=sampleFormatter

[logger_root]
level=DEBUG
handlers=consoleHandler

[logger_sampleLogger]
level=DEBUG
handlers=consoleHandler
qualname=sampleLogger
propagate=0

[handler_consoleHandler]
class=StreamHandler
level=DEBUG
formatter=sampleFormatter
args=(sys.stdout,)

[formatter_sampleFormatter]
format=%(asctime)s - %(name)s - %(levelname)s - %(message)s
```

In this example, there are:
   
    * 2 loggers
    * 1 handler
    * 1 formatter
    
Thye are configured by adding the word logger, handler and formatter
before their name (with underscore)

example:

    - logger_root
    - handler_consoleHandler
    
Here is the code that allow to load this configuration of the logger

```python
import logging
import logging.config

logging.config.fileConfig(fname='file.conf', disable_existing_loggers=False)

# Get the logger specified in the file
logger = logging.getLogger(__name__)

logger.debug('This is a debug message')
```

And here is the output it will produce

```python
2018-07-13 13:57:45,467 - __main__ - DEBUG - This is a debug message
```


     