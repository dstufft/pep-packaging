Abstract
========

The goal of this PEP is to provide a standard layout and meta-data for Python
distributions, so that all tools creating and installing distributions are
inter-operable.

To achieve this goal this PEP proposes a new format for describing meta-data
and layout of the distribution archive.


Rationale
=========

There are a number of problems currently in Python packaging.

* Lack of a standard cross tool format for distributions.
* Multiple locations where the same meta-data is defined.
* Ability to build all types of projects.


Standard Formats for Distributions
----------------------------------

Right now there are a number of competing standards for what is contained inside
of a distribution archive. distutils and setuptools share an idiom of using a
``setup.py``, distutils2 uses a ``setup.cfg``, and bento uses a ``bento.info``.

This is further compounded by the fact that due to the executable nature of
the distutils/setuptools standard ``setup.py`` distutils2 and bento can bootstrap
themselves using code located inside of ``setup.py``.


Multiple Locations for Meta-data
---------------------------------

Currently meta-data can be located in one of a minimum of two locations. ``PKG-INFO``
and ``setup.py``. It can also be located inside of ``setup.cfg``, ``bento.info``,
and any other location that a packager might wish (again due to the executable
nature of ``setup.py``).


Custom Builds
-------------

A number of projects have had to work around or monkeypatch distutils because
of assumptions that distutils makes about how to compile a project were wrong.
This includes projects that want to cross compile [#crosscompile]_ and
projects with complex compiler dependencies such as Numpy [#numpy]_.

Further more there have been serious doubts raised by some that any generic
compilation step would be able to cover all needs [#generic1]_ [#generic2]_.


What this PEP proposes
----------------------

* A new defined layout that any tool may create or consume
* A singular location to be used as the "one true source" for all meta-data
* New meta-data version to deal with new requirements.

Standard Layout
===============

All Python distributions are gzip archived containing a ``dist.json`` file
as well as any source or binary files that should be included as part of the
distribution.

Source Distribution
-------------------

A source distribution is defined as a distribution that does not include any
sort of precompiled files. A source distribution **MUST** contain a ``dist.json``
and all source files, Python or otherwise, that this distribution contains.


Binary Distribution
-------------------

A binary distribution is defined as package that does not require any sort of
compilation step to complete. A binary distribution **MUST** contain a ``dist.json``
as well as one or more directories containing a compiled distribution.


Build Directories
'''''''''''''''''

Build directories are specially named directories that signify which Platform
and Python that particular build is for.

TODO: Specify the proper naming convention for build directories.


dist.json
=========

``dist.json`` is a JSON file containing all the meta-data for this distribution.
It must be a valid JSON file and cannot be a JavaScript object literal.

Fields
------

name
''''

The most important things in your dist.json are the name and version fields. The
name and version together form an identifier that is assumed to be completely
unique. Changes to the distribution should come along with changes to the version.

The name is what your thing is called.


version
'''''''

The most important things in your dist.json are the name and version fields. The
name and version together form an identifier that is assumed to be completely
unique. Changes to the distribution should come along with changes to the version.

The version must be in the format specified in PEP 386 [#pep386]_


summary
'''''''

A one-line summary of what the distribution does.


description
'''''''''''

A longer description of the distribution that can run to several paragraphs.
Software that deals with metadata should not assume any maximum size for this
field, though people shouldn't include their instruction manual as the description.

The contents of this field can be written using reStructuredText markup [#rest].
For programs that work with the metadata, supporting markup is optional; programs
can also display the contents of the field as-is. This means that authors should
be conservative in the markup they use.


keywords
''''''''

A list of additional keywords to be used to assist searching for the distribution
in a larger catalog. It should be a list of strings.

Example::

    {
        "keywords": ["dog", "puppy", "voting election"],
    }


author
''''''

A string, dictionary representing the author of the distribution, see
`People Fields`_ for more information.


maintainer
''''''''''

A string or dictionary representing the current maintainer of the distribution,
see `People Fields`_ for more information. This field *SHOULD* be omitted if it
is the same as the author.


contributors
''''''''''''

A list of additional contributors for the distribution. Each item in the list
must either be a string or a dictionary, see `People Fields`_ for more
information.

uris
''''

A dictionary of Label: URI for this project. Each label is limited to 32 characters
in length.

Example::

    {
        "uris": {
            "Home Page": "http://python.org/",
            "Bug Tracker": "http://bugs.python.org/"
        }
    }


license
'''''''

Text indicating the license covering the distribution where the license is not
a selection from the "License" Trove classifiers. See classifiers_ below. This
field may also be used to specify a particular version of a license which is
named via the Classifier field, or to indicate a variation or exception to such
a license.


classifiers
'''''''''''

A List of strings where each item represents a distinct classifier for this
distribution. Classifiers are described in PEP 301 [#pep301]_.

Example::

    {
        "classifiers": [
            "Development Status :: 4 - Beta",
            "Environment :: Console (Text Based)"
        ]
    }


requires_python
'''''''''''''''

This field specifies the Python version(s) that the distribution is guaranteed
to be compatible with. Version numbers must be in the format specified in
`Version Specifiers`_.


People Fields
-------------

The ``author``, and ``maintainer`` fields, and the ``contributors`` field list
items each accept either a string or a dictionary. The dictionary is a mapping
of ``name``, ``email``, and ``url``, like this::

    {
        "name": "Monty Python",
        "email": "monty@python.org",
        "url": "http://python.org/"
    }

Any of the fields may be omitted where they are unknown. Additionally they
may be specified using a string in the format of ``Name <email> (url)``. An
example::

    Monty Python <monty@python.org> (http://python.org/)


References
==========

.. [#crosscompile] Cross-Compiling Python & C Extensions for Embedded Systems
   (http://pyvideo.org/video/682/cross-compiling-python-c-extensions-for-embedde)

.. [#numpy] Packaging (numpy.distutils)
   (http://docs.scipy.org/doc/numpy/reference/distutils.html)

.. [#generic1] Status of Packaging in 3.3
   (http://mail.python.org/pipermail/python-dev/2012-June/120696.html)

.. [#generic2] Status of Packaging in 3.3
   (http://mail.python.org/pipermail/python-dev/2012-June/120591.html)

.. [#rest] reStructuredText Markup
   (http://docutils.sourceforge.net/)

.. [#pep301] PEP 301 - Package Index and Metadata for Distutils
   (http://www.python.org/dev/peps/pep-0301/)

.. [#pep386] PEP 386 - Changing the version comparison module in Distutils
   (http://www.python.org/dev/peps/pep-0386)


Copyright
=========

This document has been placed in the public domain.
