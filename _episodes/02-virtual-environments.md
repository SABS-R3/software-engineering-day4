---
title: "Virtual Environments and Dependencies"
teaching: 0
exercises: 60
questions:
- "What is a virtual environment?"
- "What are the benefits of using a virtual environment?"
- "How can I test my code with a different version of Python?"
objectives:
- "Explain the features and benefits of virtual environments."
- "Use pyenv to switch between multiple versions of Python."
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

When devloping our own software, we're not working in isolation.
A huge amount of software has already been written by other people - some working as individuals, some by large groups or companies.
This means that we don't have to do everything ourselves as often, someone else has already had to solve the problem we've encountered.
For example, many people have needed to perform complex numerical calculations, so NumPy was developed to address this need.

Using an established library or package that does what we need can save us considerable time.
We don't need to take time to reimplement features that already exist - assuming the time taken to reimplement them is greater than the time taken to learn about and use the pre-existing software.

But adding a new dependency is adding risk:

- A new dependency might conflict with our other dependencies
- It might have bugs that affect the correctness of our project
- It might not work under some conditions that we don't yet know - e.g. support for other Operating Systems
- It might have security vulnerabilities
- It might not be maintained and kept up to date

Any of these issues might exist now, or may occur in the future, so we need to make sure that we have confidence in anything we're adding as a new dependency.

> ## Left-Pad
>
> In 2016 a package called `left-pad` was removed from the JavaScript package index used by the NPM package manager.
> This package, although it only contained 11 lines of code, was a dependency of several core components of the JavaScript ecosystem, meaning it had had 2.5 million downloads in the month before the removal.
> As a result, for a few hours until it was restored, JavaScript projects were broken for developers all round the world.
>
> For more information see [this article](https://www.theregister.com/2016/03/23/npm_left_pad_chaos/).
{: .callout}

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
See the pages on [choosing dependencies](https://software.ac.uk/choosing-right-open-source-software-your-project) and [avoiding dependency problems](https://software.ac.uk/resources/guides/defending-your-code-against-dependency-problems).


## Dependency Hell and Some Solutions

>
> Dependency hell is a colloquial term for the frustration of some software users who have installed software packages which have dependencies on specific versions of other software packages.
>
> The dependency issue arises around shared packages or libraries on which several other packages have dependencies but where they depend on different and incompatible versions of the shared packages.
>
> -- Wikipedia - Dependency Hell

### Semantic Versioning - SemVer

It's a good idea to add a version number to our software, so that other people can tell when we make changes, and make sure they're using the version they need.
This helps to avoid the confusion when refering to the different capabilities and behaviours - we can just say "use version 3.8.5".
One of the most common systems for numbering versions is **SemVer**.

Using SemVer, version numbers have the form `MAJOR.MINOR.PATCH`.

When a new version is released, we update the version number depending on the type of changes we made to the software.
If all we've done is fix bugs, or make changes that don't affect the functionality, we increment the third part of the version number and call this a **patch release**.
If we've added new features, but we've not broken any behaviour that people might be relying on, we increment the second number, set the third number to zero and call this a **minor release**.
If we have broken some behaviour that people might rely on, we increment the first number, set the other two to zero and call it a **major release**.

The set of behaviour that we expect people to rely on is known as our **public API**.
We should take great care to minimise changes to our public API once we know other people might be using our code.
To signify that we're still in the early stages of a project and we might have to make major changes, we can use a `0.x.y` version number.
Releases with a major version of zero, are understood to be unstable - when we want to say we're ready with a stable release, we increment to version `1.0.0`.

See the [official documentation](https://semver.org/) for more detail.

For different types of software this can mean slightly different things.
For a library, which other developers will integrate into their own software, it is the programmatic API, the set of classes, functions and data that the library exposes.
For user-facing software, such as a desktop application or web app, this can include all the previous factors, but might also include factors such as changes to user workflow.

> ## Alternatives To SemVer
>
> Ubuntu Linux uses version numbers like `19.10` and `20.04 LTS`.
>
> What do these version numbers mean?
> Why might a numbering system like this be a useful alternative to SemVer for some projects?
>
> > ## Solution
> >
> > The components in the numbering system used by Ubuntu are the year and month.
> > Version `19.10` was released around October 2019, while version `20.04 LTS` was released around April 2020.
> > The `LTS` is short for **Long Term Support**, meaning that they commit to maintaining this version for longer than non-LTS versions.
> > Using an LTS version is useful if you require stability, more than you require the new features of upgrading to a new version.
> > It's common for LTS versions of software to be maintained for 3-5 years, or even longer.
> >
> > The purpose of a numbering system based on date, is that it creates a known **release cadence**.
> > Users of the software know immediately if it's time for them to update, and how long ago the current version was released.
> > It does however, mean that we don't know how major the changes in each version are, until we read the **release notes**.
> {: .solution}
{: .challenge}

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

### Requirements Files

If we're going to be adding dependencies to our project, we need a way to tell people what these are.
When we share our code with someone else, they'll need to be able to install the same things, and possibly even the same versions of the same things.
Pip has some more features which help us manage this.

Firstly, to list the packages we have installed, we can use the list command:

~~~
pip3 list
~~~
{: .language-bash}

If you don't see any listed packages, you're probably either in a new virtual environment, or not in a virtual environment at all.
Since we've been installing everything so far, in virtual environments, there shouldn't be anything installed in your main version of Python.
Try activating one of our previous virtual environments, or creating a new one and installing a few packages to see if they show up now.

List is useful if we want to check if we remembered to install a particular package, but we have another command that's more useful for sharing our package list:

~~~
pip3 freeze
~~~
{: .language-bash}

The freeze command shows us all of our installed packages with the version numbers.
By saving this into a file, we can make sure that other people using our code have the same versions installed:

~~~
pip3 freeze > requirements.txt
~~~
{: .language-bash}

If you open this `requirements.txt` file using VSCode, you should see that it contains the output of `pip3 freeze`.

When we need to install all our dependencies, we can get pip to use this file:

~~~
pip3 install -r requirements.txt
~~~
{: .language-bash}

### Different Python Versions

Sometimes, particularly if we're developing a larger piece of software, it can be useful to be able to test with multiple different versions of Python.
This is where pyenv comes in.
Pyenv is a tool which allows us to have multiple versions of Python installed in parallel, and switch between them easily.
For an explanation of how it works, and the differences between pyenv an a virtual environment tool, see [their documentation](https://github.com/pyenv/pyenv).

Before we go any further, let's make sure we don't have any virtual environments currently activated.
If we don't currently have a virtual environment activated this will give us an error message - that's fine.

~~~
deactivate
~~~
{: .language-bash}

The recommended install method on Linux is by downloading and running a Bash script from their GitHub repository.
To do this from the command line, we can use a tool called `curl`.
Since `curl` may not be installed by default, we should install it ourselves using the Ubuntu package manager `apt-get`.
We'll also need an SSL library (helps us manage securely connecting to the internet) to install new versions of Python later, so we'll install that now as well.
When we do `sudo apt-get`, we need to enter our password, so the computer can check that we do actually have permission to install software.
Once we've installed `curl`, then we can install pyenv:

~~~
sudo apt-get update
sudo apt-get install curl libssl-dev
curl -sSL https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
~~~
{: .language-bash}

> ## Security Alert!
>
> Be very careful running scripts you download from the internet.
> Particularly if the script has to be run with `sudo` (this one doesn't).
> The `sudo` command runs a program with admin permissions, so handle with care.
{: .callout}

There's one last step we need to do - add pyenv to our PATH.
The PATH (or just path) is the list of directories that our shell will look in when we ask it to find and run a program for us.
Since pyenv installs into a new directory that it created itself, we need to add that directory to the PATH, before Bash can find pyenv for us.

To do this, we need to add a couple of lines to the bottom of our `.bashrc` file.
The easiest way to find this file is to open it in VSCode from the terminal:

~~~
cd
code .bashrc
~~~
{: .language-bash}

The lines we need to add to the bottom are:

~~~
export PATH="/home/sabsr3/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
~~~
{: .source}

Take care that these lines are correct before saving the file.
Now close and reopen your terminal, so that it can get the new configuration.
With the installation process completed, we should now be able to find pyenv:

~~~
which pyenv
~~~
{: .language-bash}

~~~
/home/sabsr3/.pyenv/bin/pyenv
~~~
{: .output}

Let's say we want to try out the latest development version of Python (3.10-dev at the time of writing), we can install this with pyenv.
Be aware that this might take a couple of minutes to complete, as it's downloading and configuring a full installation of a new version of Python.

~~~
pyenv install 3.10-dev
~~~
{: .language-bash}

Now, in our code directory, we can tell pyenv which version we want to use:

~~~
cd
cd 2020-se-day4/code
pyenv local 3.10-dev
~~~
{: .language-bash}

Now, whenever we use Python in this directory, it will be our new Python 3.10 installation.
We can even use this Python to create a new virtual environment.

We can test this by asking Python what its version number is:

~~~
python --version
~~~
{: .language-bash}

~~~
Python 3.10.0a1+
~~~
{: .output}

> ## Python Version Numbers
>
> At the time these materials were prepared, installing `3.10-dev` got us a version of Python with a version number of `3.10.0a1+`.
> What does this version number mean?
> When we share our code, should we tell people to use this version?
> Why, or why not?
>
> > ## Solution
> >
> > The version number `3.10.0a1+` has a few more components than we've seen previously:
> >
> > - The major version is `3`
> > - The minor version is `10`
> > - The patch version is `0` - this is part of the first proper release in the `3.10` series
> > - `a1` means that this is the first **alpha release** in this series
> > - `+` means that this is a development version and some changes have been made since the actual `3.10.0a1` release
> >
> > Before a stable release (one which is considered reliable), we often release alpha and beta versions.
> > An alpha or beta version allows people to try out some of the new things we've added, but is a warning that it should not be relied on.
> >
> > Installing a development version is when we ignore the officially released versions and go directly to the latest changes in version control.
> > These are very dangerous to rely on, as anything could potentially be broken at any time, though many larger project try to keep them as stable as possible.
> > The advantage of a development version is it allows us to try out the very latest additions, and report any bugs that we find.
> >
> > Because this is an alpha release, but we're also using the very latest code, we should not say that this is a required version of Python.
> > If we want to use some of the functionality in Python 3.10, we should wait for the full `3.10.0` release before sharing our instructions.
> {: .solution}
{: .challenge}


{% include links.md %}
