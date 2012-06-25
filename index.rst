Abstract
========

The goal of this PEP is to provide a standard layout and format for Python
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


Standard Format for Distributions
---------------------------------

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


Copyright
=========

This document has been placed in the public domain.
