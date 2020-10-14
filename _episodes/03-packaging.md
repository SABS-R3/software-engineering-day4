---
title: "Packaging Code for Release"
teaching: 0
exercises: 90
questions:
- "How do we prepare our code for sharing as a Python package?"
- "How do we release our project for other people to install?"
objectives:
- "Describe the steps necessary for sharing Python code as installable packages."
- "Use Poetry to prepare an installable package."
- "Explain the differences between runtime and development dependencies."
- "Upload an installable Python package to a package index."
keypoints:
- "Poetry allows us to produce an installable package and upload it to a package index."
- "Making our software installable with pip makes it easier for others to start using it."
- "For complete control over building a package, we can use a setup.py file."
---

## Where do Packages Come From?

All the dependencies we've installed previously with `pip` were written by someone else and uploaded to a **Package Index / Repository**.

In this session we'll be looking at two different methods for building an installable package from our code.
The first, using a tool called **Poetry**, is the simpler of the two methods, so we'll walk through the complete packaging process and end up with a package uploaded to the PyPI test site.
The second method, using **setup.py**, is the more traditional method and gives us full control, but is more complicated.

## Packaging our Code with Poetry

### Installing Poetry

The recommended install method for Poetry is similar to the method we used for pyenv.
This time it's a Python script we need to download and run:

~~~
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python3
~~~
{: .language-bash}

While Poetry is installing, it lets us know that we might have to run an extra command before we can use it.
Once it's finished installing, the simplest thing to do here is to just close our terminal and open another one.
Then the changes that Poetry makes should have been applied automatically for us.

To test, we can ask where Poetry is installed:

~~~
which poetry
~~~
{: .language-bash}

~~~
/home/sabsr3/.poetry/bin/poetry
~~~
{: .output}

If you don't get this output, then we should try activating it manually and checking again:

~~~
source $HOME/.poetry/env
which poetry
~~~
{: .language-bash}

~~~
/home/sabsr3/.poetry/bin/poetry
~~~
{: .output}

### Setting up our Poetry Config

Poetry uses a **pyproject.toml** file to describe the build system and requirements of the package.
This file format was described in [PEP 518](https://www.python.org/dev/peps/pep-0518/) to solve problems with bootstrapping (the processing we do to prepare to process something) packages using the older convention **setup.py** files and to support a wider range of build tools.

Because we're going to use Poetry to manage our dependencies for us, lets deactivate, remove and remake our virtual environment to make sure it's clean.
First make sure we're in today's `code` directory, then use the following commands:

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
Note that, usually when we're naming a Python installable package, we use hyphens to separate words.

When we get to the questions about defining our dependencies, we'll answer no, so we can do this separately later.

~~~
poetry init
~~~
{: .language-bash}

~~~

This command will guide you through creating your pyproject.toml config.

Package name [example]:  poetry-project-jgraham
Version [0.1.0]: 0.1.0
Description []:  Example project for using Poetry to build packages
Author [None, n to skip]: James Graham <J.Graham@software.ac.uk>
License []:  MIT
Compatible Python versions [^3.8]: ^3.8

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
requires = ["poetry>=1.0.0"]
build-backend = "poetry.core.masonry.api"


Do you confirm generation? (yes/no) [yes] yes
~~~
{: .output}


### Project Dependencies

Previously, we looked at using a `requirements.txt` file to define the dependencies of our software.
Here, Poetry takes inspiration from package managers in other languages, particularly NPM (Node Package Manager), often used in JavaScript.

Tools like Poetry and NPM understand that there's two different types of dependency: runtime dependencies and development dependencies.
Runtime dependencies are those dependencies that need to be installed for our code to run, like NumPy.
Development dependencies are dependencies which are an essential part of your development process for a project, but are not required to run it.
Common examples of developments dependencies are linters and test frameworks, like Pylint or Pytest.

When we add a dependency using Poetry, Poetry will add it to the list of dependencies in the `pyproject.toml` file, add a reference to it in a new `poetry.lock` file, and automatically install the package into our virtual environment.
The `pyproject.toml` file has two separate lists, allowing us to distinguish between runtime and development dependencies.

~~~
poetry add numpy
poetry add --dev pylint
~~~
{: .language-bash}

### Packaging Our Code

Next, we need to make sure that our code is organised in the recommended Python code package structure.
This is the package (yes, we use the same word to mean two different things...) structure that we encountered in the refactoring section - a directory containing an `__init__.py` and our Python source code files.

We've provided an example using the academic model in the code directory for today, but you'll have to rename this package so that it matches the name you told Poetry about, but with underscores instead of hyphens.
By convention installable package (the type we install with `pip`) names use hyphens, whereas code package (a directory of Python files) names use underscores.
While we could choose to use underscores in an installable package name, we cannot use hyphens in a code package name, as Python will interpret them as a minus sign.

~~~
mv poetry_project poetry_project_jgraham
~~~
{: .language-bash}

Once we've got out `pyproject.toml` configuration done and our code in the right structure, we can go ahead and build a distributable version of our software:

~~~
poetry build
~~~
{: .language-bash}

This should produce two files for us in the `dist` directory.
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

Firstly, we need to create an account at https://test.pypi.org/.
Click the register link to the top right, and fill in your account details.
Once you've created your account, you may receive an email asking you to verify the account creation.

Now, we need to tell Poetry about our account on the test PyPI server.
When we enter the second of the following commands, Poetry will also ask us to enter our password:

~~~
poetry config repositories.testpypi https://test.pypi.org/legacy/
poetry config http-basic.testpypi <your username>
~~~
{: .language-bash}

Finally, we're ready to go.
To publish the our software that we've been working so hard on, there's just one more command:

~~~
poetry publish -r testpypi
~~~
{: .language-bash}

If we now go to https://test.pypi.org and search for our package name, we should find our newly published software!
We can even install this package ourselves using `pip`.

...

> ## Adding A Bit Of Detail
>
> Using the [Poetry documentation](https://python-poetry.org/docs/), investigate how we might go about adding more detail to our page on the package index website.
> We want people to be able to find our package and to be able to tell if it's going to be useful to them.
> What extra information can we add to the `pyproject.toml` file to help with this?
{: .challenge}

## What If We Need More Control?

Sometimes we need more control over the process of building our installable package than Poetry allows.
In these cases, we have to use the method that existed before Poetry - a `setup.py` file.

...

{% include links.md %}
