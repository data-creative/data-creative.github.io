---
layout: post
title:  "Publishing a Python Package to the Python Package Index (PyPI)"
author: MJ Rossetti
published: true
img: python-logo.png
repo_url: https://github.com/s2t2/game-utils-py
project_url: https://pypi.org/project/s2t2-game-utils/
categories:
 - reference-docs
technologies:
 - python
 - pip
 - twine
 - anaconda
 - pypi
 - setuptools
credits:
 - https://realpython.com/pypi-publish-python-package/
 - https://packaging.python.org/guides/distributing-packages-using-setuptools/
---

This document describes the process of building and releasing a Python Package to the Python Package Index (PyPI).

The [first Python package I released](https://pypi.org/project/s2t2-game-utils/) contains logic to play a game of ["Rock-Paper-Scissors"](https://en.wikipedia.org/wiki/Rock%E2%80%93paper%E2%80%93scissors).

After a package is released to the PyPI, we can use `pip` to install it:

```sh
pip install s2t2-game-utils
```

So we can import and use it in other programs:

```py
from game_utils.rock_paper_scissors import determine_winner

determine_winner("rock", "paper") #> "paper"
```

## Repo Structure

Here is the structure of the Python package's repository:

```
my-repo/
│
├── game_utils/
│   ├── __init__.py
│   └── rock_paper_scissors.py
│
├── test/
│   ├── rock_paper_scissors_test.py
│
├── conftest.py
├── LICENSE.md
├── README.md
└── setup.py
```

And importantly, the contents of "setup.py" (which will be used to configure the build):

```py
# setup.py

from setuptools import find_packages, setup

with open("README.md", "r") as fh:
    long_description = fh.read()

setup(
    name="s2t2-game-utils",
    version="1.0",
    author="MJ Rossetti",
    author_email="datacreativellc@gmail.com",
    description="Gameplay logic for Rock-Paper-Scissors",
    long_description=long_description,
    long_description_content_type="text/markdown", # required if using a md file for long desc
    license="MIT",
    url="https://github.com/s2t2/game-utils-py",
    keywords="rock paper scissors game",
    packages=find_packages() # ["game_utils"]
)
```

## Build and Release Process

### Prerequisites

  + Anaconda 3.7, Python 3.7, Pip
  + Sign up for a [PyPI Account](https://pypi.org)
  + Sign up for a [PyPI Test Server Account](https://test.pypi.org)

### Setup

Create and activate a virtual environment:

```sh
conda create -n twine-env python=3.7 # (first time only)
conda activate twine-env

pip install twine # (first time only)
```

### Building

Generate / build package distribution files:

```sh
python setup.py sdist bdist_wheel
```

Check the distribution to ensure the "setup.py" file has been configured properly:

```sh
twine check dist/*
```

If there is an error in the distribution, you'll need to re-generate it and re-check it.

### Releasing

Otherwise, push to the PYPI Test Server:

```sh
twine upload --skip-existing --repository-url https://test.pypi.org/legacy/ dist/*
```

Finally, if everything looks good, release to PyPI:

```sh
twine upload --skip-existing dist/*
```

Oh yeah :-D

![A screenshot of the package on the python package index](/assets/img/posts/rock-paper-scissors-pypi.png)
