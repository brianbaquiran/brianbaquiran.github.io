.. title: Notes on Developing Python Packages
.. slug: notes-on-developing-python-packages
.. date: 2016-02-02 08:43:39 UTC+08:00
.. tags:
.. category:
.. link:
.. description:
.. type: text

Be lazy. Use `cookiecutter`_ and initialize your project with one of the template projects.

Directory structure
===================

Suppose we're developing a package named ``teacup``. The minimalist directory structure should look like this::

  teacup/
    teacup/
      __init__.py
      teacup.py
    setup.py
    .gitignore

The top level ``teacup`` directory will be your git working directory. The second level ``teacup`` directory is a python package, and ``teacup.py`` is the main module for the package.

The ``setup.py`` file is the usual setuptools package configuration.

A more complete directory structure would be::

  teacup/
    docs/
    tests/
    teacup/
      __init__.py
      teacup
    setup.py
    .gitignore
    README.rst

Note that ``tests`` is outside the ``teacup`` package. This avoids the tests being included when distributing the package. ``docs`` would be your `Sphinx`_ documentation repository.

What to place in __init__.py
============================
Read `Be Pythonic: __init__.py <http://mikegrouchy.com/blog/2012/05/be-pythonic-__init__py.html>`_ for an overview of how it works.
Read this `Reddit thread`_ on how it's actually used in practice.

.. tip:: Quiz yourself:
  What are the three different ways __init__.py is commonly used? Which one do you prefer?

From standalone module to package
=================================
It's common to start with your code in a module, but later realize it should be better organized in a package structure.
If you already have other applications using your module, you can use ``__init__.py`` to maintain API compatibility with these other applications.

For example, if we have an existing module ``silverware``::

  # silverware.py
  def teaspoon():
    pass

  def knife():
    pass

  def fork():
    pass


There are already applications and other libraries using our functions ``teaspoon``, ``knife`` and ``fork``::

  # this is dinnertable.py, an app or module that uses our silverware module
  from silverware import teaspoon

  teaspoon()

If we want to turn the ``silverware`` module into a package, possibly containing other modules we would structure it as follows::

  silverware/
    silverware/
      __init__.py
      silverware.py
      spoons.py
      forks.py
    setup.py
    .gitignore

To maintain API compatibility with the applications and modules that were already using ``silverware``, we place the following in ``__init__.py``::

    # __init__.py of the silverware package
    from silverware.spoons import teaspoon
    from silverware.forks import fork
    from silverware.silverware import knife

This effectively places teaspoon, knife and fork in the `silverware` namespace and allows ``dinnertable.py`` to still use ``from silverware import teaspoon`` even if ``teaspoon`` is defined in the ``silverware.spoons`` module and ``fork`` in the ``silverware.forks`` module.

.. note:: I used absolute imports in this example. You could, conceivably, use relative imports like this::

    # __init__.py of the silverware package
    from spoons import teaspoon
    from forks import fork
    from silverware import knife

  And it would work just fine. But explicit is better than implicit.

.. _cookiecutter:  http://cookiecutter.readthedocs.org
.. _Reddit thread: https://www.reddit.com/r/Python/comments/1bbbwk/whats_your_opinion_on_what_to_include_in_init_py/
.. _Sphinx: http://docs.writethedocs.org/tools/sphinx/
