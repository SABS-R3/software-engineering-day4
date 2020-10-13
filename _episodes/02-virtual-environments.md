---
title: "Using Virtual Environments"
teaching: 0
exercises: 40
questions:
- "What is a virtual environment?"
- "What are the benefits of using a virtual environment?"
objectives:
- "Explain the features and benefits of virtual environments."
- "Outline the steps necessary for sharing code with others."
keypoints:
- "Virtual environments help us to isolate our dependencies from the system."
---

## Virtual Environments and Managing Dependencies

See topic [video lecture](https://www.youtube.com/watch?v=hDSKafGlgWI), and [PowerPoint slides](../slides/3.1-Development-Practices.pptx) used with per-slide notes.

### Why do we have Dependencies?

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

## Dependency Hell and Some Solutions

>
> Dependency hell is a colloquial term for the frustration of some software users who have installed software packages which have dependencies on specific versions of other software packages.
>
> The dependency issue arises around shared packages or libraries on which several other packages have dependencies but where they depend on different and incompatible versions of the shared packages.
>
> -- Wikipedia - Dependency Hell

### Semantic Versioning - SemVer

See https://semver.org/

Version numbers have the form `MAJOR.MINOR.PATCH`.

When a new version is released, we update the version number depending on the type of changes we made to the software.
If all we've done is fix bugs, or make changes that don't affect the functionality, we increment the third part of the version number and call this a **patch release**.
If we've added new features, but we've not broken any behaviour that people might be relying on, we increment the second number, set the third number to zero and call this a **minor release**.
If we have broken some behaviour that people might rely on, we increment the first number, set the other two to zero and call it a **major release**.

The set of behaviour that we expect people to rely on is known as our **public API**.

For different types of software this can mean slightly different things.
For a library, which other developers will integrate into their own software, it is the programmatic API, the set of classes, functions and data that the library exposes.

### Virtual Environments

We've used virtual environments previously when we had to install dependencies, but not properly discussed how they work.

When we install Python packages

### Requirements Files

`requirements.txt`

### Different Python Versions

`pyenv`

- Fork of rbenv
- https://github.com/pyenv/pyenv


{% include links.md %}
