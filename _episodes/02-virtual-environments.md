---
title: "Virtual Environments and Dependencies"
teaching: 0
exercises: 60
questions:
- "What is a virtual environment?"
- "What are the benefits of using a virtual environment?"
objectives:
- "Explain the features and benefits of virtual environments."
- "Outline the steps necessary for sharing code with others."
keypoints:
- "Virtual environments help us to isolate our dependencies from the system."
---

## Virtual Environments and Dependencies

See topic [video lecture](https://www.youtube.com/watch?v=hDSKafGlgWI), and [PowerPoint slides](../slides/4.1-Virtual-Environments-and-Packaging.pptx) used with per-slide notes.

## Set up training materials

So let's download the training materials for this material from the GitHub code repository online. Go to [https://github.com/SABS-R3/2020-software-engineering-day4/tree/gh-pages](https://github.com/SABS-R3/2020-software-engineering-day4/tree/gh-pages) in a browser (any will do, although Firefox is already installed on the provided laptops). Select the green `Code` button, and then select `Download ZIP`, and then in Firefox selecting `Save File` at the dialogue prompt. This will download all the files within a single archive file. After it's finished downloading, we need to extract all files from the archive. Find where the file has been downloaded to (on the provided laptops this is `/home/sabsr3/Downloads`, then start a terminal. You can start a terminal by right-clicking on the desktop and selecting `Open in Terminal`. Assuming the file has downloaded to e.g. `/home/sabsr3/Downloads`, type the following within the Terminal shell:

~~~
cd ~
unzip /home/sabsr3/Downloads/2020-software-engineering-day4-gh-pages.zip
~~~
{: .language-bash}

The first `cd ~` command *changes our working directory* to our home directory (on the provisioned laptops, this is `/home/sabsr3`).

The second command uses the unzip program to unpack the archive in your home directory, within a subdirectory called `2020-software-engineering-day4-gh-pages`. This subdirectory name is a little long to easily work with, so we'll rename it to something shorter:

~~~
mv 2020-software-engineering-day4-gh-pages 2020-se-day4
~~~
{: .language-bash}

Change to the `code` directory within that new directory:

~~~
cd 2020-se-day3/code
~~~
{: .language-bash}

## Why do we have Dependencies?

- Don't need to write everything ourselves
- If there's an established library / package that does what we need consider using it
- But adding a new dependency is adding risk
  - A new dependency might conflict with our other dependencies
  - It might have bugs that affect the correctness of our project
  - It might not work under some conditions that we don't yet know - e.g. support for other Operating Systems
  - It might have security vulnerabilities
  - It might not be maintained and kept up to date

Any of these issues might exist now, or may occur in the future, so we need to make sure that we have confidence in anything we're adding as a new dependency.

### Assessing Dependencies

Most of the things we're looking at in this course are useful things you should be looking for in a dependency.
Particularly, we tend to look for:

- Good documentation
- Good automated testing, with good test coverage
- Good use of project management tools such as issue tracking
- A continuous history of updates, where appropriate

From the documentation, we're particularly looking for guidance on how we should be integrating it into our own code.
The API documentation we prepared with docstrings and Sphinx is especially useful as, if it's done well, each component of the software should have detailed instructions

The Software Sustainability Institute produced some guides that may be useful when you're thinking about adding a new dependency.
They're a bit dated, but most of the points are still valid.
See the pages on [choosing dependencies](https://software.ac.uk/choosing-right-open-source-software-your-project) and [avoiding dependency problems](https://software.ac.uk/resources/guides/defending-your-code-against-dependency-problems#node-252).


## Dependency Hell and Some Solutions

>
> Dependency hell is a colloquial term for the frustration of some software users who have installed software packages which have dependencies on specific versions of other software packages.
>
> The dependency issue arises around shared packages or libraries on which several other packages have dependencies but where they depend on different and incompatible versions of the shared packages.
>
> -- Wikipedia - Dependency Hell

### Semantic Versioning - SemVer

See the [official documentation](https://semver.org/) for more detail.

Version numbers have the form `MAJOR.MINOR.PATCH`.

When a new version is released, we update the version number depending on the type of changes we made to the software.
If all we've done is fix bugs, or make changes that don't affect the functionality, we increment the third part of the version number and call this a **patch release**.
If we've added new features, but we've not broken any behaviour that people might be relying on, we increment the second number, set the third number to zero and call this a **minor release**.
If we have broken some behaviour that people might rely on, we increment the first number, set the other two to zero and call it a **major release**.

The set of behaviour that we expect people to rely on is known as our **public API**.

For different types of software this can mean slightly different things.
For a library, which other developers will integrate into their own software, it is the programmatic API, the set of classes, functions and data that the library exposes.

### Virtual Environments

We've used virtual environments previously when we had to install dependencies, but not properly discussed what they are and how they work.

Consider developing a number of different Python projects that each have their own package dependencies (and versions of those dependencies) on the same machine.
It could quickly become confusing as to which packages and package versions are required by each project, making it difficult for others to run your scripts themselves (or yourself on another machine!).
Additionally, different scripts may need to use different versions of the same package.

A Python virtual environment is an isolated working copy (you can think of it as a self-contained directory tree) of a specific version of Python together with specific versions of a number of installed packages which allows you to work on a particular project without worry of affecting other projects.
In other words, it enables multiple side-by-side installations of Python and different versions of the same third party package to coexist on your machine and only one to be selected for each of your projects.
As more dependencies are added to your Python project over time, you can add them to this specific virtual environment and avoid a great deal of confusion by having separate virtual environments for each project.

Here are some typical virtual environment use cases:

- You have an older project that only works under Python 2.
  You do not have the time to migrate the project to Python 3 or it may not even be possible as some of the third party dependencies are not available under Python 3.
  You have to start another project under Python 3.
  The best way to do this on a single machine is to set up 2 separate Python virtual environments.
- One of your projects is locked to use a particular older version of a third party dependency.
  You cannot use the latest version of the dependency as it breaks things in your project.
  In a separate branch of your project, you want to try and fix problems introduced by the new version of the dependency without affecting the working version of your project.

You do not have to worry too much about specific versions of packages that your project depends on most of the time. Virtual environments enable you to always use the latest available version without specifying it explicitly. They also enable you to use a specific older version of a package for your project, should you need to.

When we install Python packages

### Requirements Files

`requirements.txt`

If we're going to be adding dependencies to our project, we need a way to tell people what these are.
When we share our code with someone else, they'll need to be able to install the same things, and possibly even the same versions of the same things.

Pip comes with some features to help us with this

### Different Python Versions

`pyenv`

- Fork of rbenv
- https://github.com/pyenv/pyenv


{% include links.md %}
