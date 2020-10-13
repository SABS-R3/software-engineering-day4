---
title: "Packaging Code for Release"
teaching: 0
exercises: 80
questions:
- "How do we release our code?"
- "How do we prepare our code for sharing?"
objectives:
- "Explain the features and benefits of virtual environments."
- "Outline the steps necessary for sharing code with others."
keypoints:
- "."
---

## Where do Packages Come From?

All the packages we've installed previously, whether using `pip` or `conda`, were written by someone and uploaded to a **Package Index / Repository**

We'll look at two different methods for packaging our code.
The first, using a tool called **Poetry**, is the simpler of the two methods, so we'll walk through the complete packaging process and end up with a package uploaded to the PyPI test site.
The second method, using **setup.py**, is the more traditional method and gives us full control, but is more complicated.

## Packaging our Code with Poetry

### Installing Poetry

The recommended install method on Linux is:
```curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python```

If that fails:
```pip3 install --user poetry```

### Setting up our Poetry Config

Poetry uses a **pyproject.toml** file to describe the build system and requirements of the package.
This file was described in [PEP 518](https://www.python.org/dev/peps/pep-0518/) to solve problems with bootstrapping packages using the older convention **setup.py** files and to support a wider range of build tools.

To create a `pyproject.toml` file for our code, we can use `poetry init`.
This will guide us through the most important settings - for each prompt, we either enter our data or accept the default.

~~~
poetry init
~~~
{: .language-bash}

~~~

This command will guide you through creating your pyproject.toml config.

Package name [example]:  example-project
Version [0.1.0]:
Description []:  Example project for using Poetry to build packages
Author [James Graham <J.Graham@software.ac.uk>, n to skip]:
License []:  MIT
Compatible Python versions [^3.7]:

Would you like to define your main dependencies interactively? (yes/no) [yes] no
Would you like to define your development dependencies interactively? (yes/no) [yes] no
Generated file

[tool.poetry]
name = "example-project"
version = "0.1.0"
description = "Example project for using Poetry to build packages"
authors = ["James Graham <J.Graham@software.ac.uk>"]
license = "MIT"

[tool.poetry.dependencies]
python = "^3.7"

[tool.poetry.dev-dependencies]

[build-system]
requires = ["poetry>=0.12"]
build-backend = "poetry.masonry.api"


Do you confirm generation? (yes/no) [yes] yes
~~~
{: .output}


### Project Dependencies

Previously, we looked at using a `requirements.txt` file to define the dependencies.
Here, Poetry takes inspiration from package managers in other languages, particularly NPM (Node Package Manager), often used in JavaScript.

~~~
poetry add numpy
poetry add -D pytest
~~~
{: .language-bash}


## What If We Need More Control?

`setup.py`

- Useful if you need to control the build process - e.g. to compile some C / C++ components


{% include links.md %}
