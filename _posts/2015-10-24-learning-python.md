---
layout: post
title:  "Learning Python"
author: MJ Rossetti
categories:
 - posts
 - process-documentation
img: python-logo.png
tags: python pip
published: true
icon_class: snake
technologies:
 - python
 - pip
credits:  
 - http://stackoverflow.com/questions/8726207/what-are-the-python-equivalents-to-rubys-bundler-perls-carton
 - http://pip.readthedocs.org/en/stable/user_guide/#installing-packages
 - http://pip.readthedocs.org/en/stable/reference/pip_install/#requirements-file-format
 - https://pip.readthedocs.org/en/1.1/requirements.html#the-requirements-file-format
 - http://stackoverflow.com/questions/510972/getting-the-class-name-of-an-instance-in-python
 - https://docs.python.org/2/library/stdtypes.html#mapping-types-dict
 - https://docs.python.org/2/library/stdtypes.html#sequence-types-str-unicode-list-tuple-bytearray-buffer-xrange
 - https://developers.google.com/edu/python/dict-files?hl=en
---

## Prerequisites

Install python. If using homebrew on Mac OS X: `brew install python`, and follow post-installation instructions.

Create a new project directory.

```` sh
mkdir ~/Desktop/myApp
cd ~/Desktop/myApp
````

## Python Package Management

Specify package dependencies, if necessary.

Add a file called **requirements.txt** to the root directory. Write the name of each required python package dependency on a new line, save the file, and exit.

    numpy
    django
    git+https://github.com/eskerda/pybikes.git

Install package dependencies, if necessary.

```` sh
pip install -r requirements.txt
````

## Executing Python Scripts

Add a file called **my_script.py** to the root directory.

    # my_script.py
    print("DOING WORK HERE")

Run it.

```` sh
python my_script.py
````

## Python Debugging

Debug the script as necessary.

```` py
# https://gist.github.com/obfusk/208597ccc64bf9b436ed
import code
code.interact(local=locals())
````

## Python Objects and Methods

Detect classes of objects.

```` py
type(my_obj) # or...
my_obj.__class__.__name__
````

Detect class methods and documentation.

```` py
dir(my_obj)
help(my_obj)
````

### Python Object Types

`dict`, or dictionary, is like a hash

`u`, or unicode, is like a string

Convert `dict` to json object:

```` py
json.dumps(my_obj)
````

## Python For Each Loops

```` py
# http://www.tutorialspoint.com/python/python_for_loop.htm
fruits = ['banana', 'apple',  'mango']
for fruit in fruits:
  print 'Current fruit :', fruit
````

## Python Rescue from Exceptions

```` py
try:
  network_name = response.meta["name"].encode()
except UnicodeEncodeError:
  network_name = "#UNENCODABLE"
except UnicodeDecodeError:
  network_name = "#UNDECODABLE"
````
