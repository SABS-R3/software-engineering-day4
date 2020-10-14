---
title: "Packaging Code for Release"
teaching: 0
exercises: 90
questions:
- "How do we prepare our code for sharing?"
- "How do we release our code?"
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

Because we're going to use Poetry to manage our dependencies for us, lets deactivate, remove and remake our virtual environment to make sure it's clean:

~~~
deactivate
rm -r venv
python3 -m venv venv
source venv/bin/activate
~~~
{: .language-bash}

Now we're ready to begin.

To create a `pyproject.toml` file for our code, we can use `poetry init`.
This will guide us through the most important settings - for each prompt, we either enter our data or accept the default.
For the package name, make sure this has some unique identifier in it so it doesn't match the package name of anyone else - I've used my name here, you could use your name, or some random text.
This is because, if we want to upload it to the test version of PyPI at the end, it will need to be globally unique.

~~~
poetry init
~~~
{: .language-bash}

~~~

This command will guide you through creating your pyproject.toml config.

Package name [example]:  poetry-project-jgraham
Version [0.1.0]:
Description []:  Example project for using Poetry to build packages
Author [James Graham <J.Graham@software.ac.uk>, n to skip]: James Graham <J.Graham@software.ac.uk>
License []:  MIT
Compatible Python versions [^3.7]:

Would you like to define your main dependencies interactively? (yes/no) [yes] no
Would you like to define your development dependencies interactively? (yes/no) [yes] no
Generated file

[tool.poetry]
name = "poetry-project-jgraham"
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

Tools like Poetry and NPM understand that there's two different types of dependency: runtime dependencies and development dependencies.
Runtime dependencies are those dependencies that need to be installed for our code to run, like NumPy.
Development dependencies are dependencies which are an essential part of your development process for a project, but are not required to run it.
Common examples of developments dependencies are Pylint or Pytest.

Once we've created and activated our virtual environment, we can add the dependencies via Poetry.
When we add a dependency this way, Poetry will add it to the list of dependencies in the `pyproject.toml` file, add a reference to it in a new `poetry.lock` file, and automatically install the package into our virtual environment.

~~~
poetry add numpy
poetry add --dev pylint
~~~
{: .language-bash}

### Packaging Our Code

Next, we need to make sure that our code is organised in the recommended Python package structure.
This is the package (yes, we use the same word to mean two different things...) structure that we encountered in the refactoring section - a directory containing an `__init__.py` and our Python source code files.

We've provided an example using the academic model in the code directory for today, but you'll have to rename this package so that it matches the name you told Poetry about, but with underscores instead of hyphens.
By convention package (the type with install with `pip`) names use hyphens, whereas package (a directory of Python files) names can only use underscores.

~~~
mv poetry_project poetry_project_jgraham
~~~
{: .language-bash}

Once we've got out `pyproject.toml` configuration done and our code in the right structure, we can go ahead and build a distributable Python package (the installable kind):

~~~
poetry build
~~~
{: .language-bash}

This should produce two files for you in the `dist` directory.
The one we care most about is the `.whl` or **wheel** file.
This is the file that `pip` uses to distribute and install Python packages, so this is the file we'd need to share with other people who want to install our software.

To install this wheel file, we can give it to `pip`:

~~~
pip3 install dist/poetry_project*.whl
~~~
{: .language-bash}

The star in the line above is a **wildcard**, that means Bash should use any filenames that match that pattern, with any number of characters in place for the star.
We could also rely on Bash's autocomplete functionality and type `dist/poetry_project`, then hit the <kbd>Tab</kbd> key.

### Sharing Our Package With The World

The final step in distributing our code is to upload it to a package index.
To help us test our packages, PyPI provides a test index at https://test.pypi.org/ that we can use.
Packages uploaded to the test index aren't accessible to a normal `pip install`, so no one's going to accidentally install our software yet.


## What If We Need More Control?

`setup.py`

- Useful if you need to control the build process - e.g. to compile some C / C++ components


{% include links.md %}
