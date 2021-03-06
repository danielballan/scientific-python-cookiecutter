===================
Publishing Releases
===================

In this section you will:

* Tag a release of your Python package.
* Upload it to `PyPI <https://pypi.org/>`_ (Python Package Index) so that
  users can download and install it using pip.

#. Choose a version number. The convention following by most scientific Python
   packages is ``vMAJOR.MINOR.MICRO``, as in ``v1.3.0``. A good number to start
   with is ``v0.1.0``.

   These numbers have meanings.
   The goal is to communicate to the user whether upgrading will break anything
   in *their* code that will need to be updated in turn.
   
   * Incrementing the ``MICRO`` number (``v1.3.0`` -> ``v1.3.1``) means, "I
     have fixed some bugs, but I have not added any major new features or
     changed any names. All of your code should still work without changes."
   * Incrementing the ``MINOR`` number (``v1.3.0`` -> ``v1.4.0``) means, "I
     have added some new features and changed some minor things. Your code
     should work with perhaps some small updates."
   * Incrementing the ``MAJOR`` number (``v1.3.0`` -> ``v2.0.0``) means, "I
     have made major changes that will probably require you to update your
     code."

   Additionally, if the ``MAJOR`` version is ``0``, the project is considered
   to be in early development and may make major breaking changes in minor
   releases.

   Obviously this is an imprecise system. Think of it a highly-compressed,
   lossy representation of how painful it will be for the user to upgrade.

#. Update ``release-history.rst`` in the documentation if you have not done so
   already. (See :doc:`writing-docs`.) It must address any breaking changes
   that could require users of your Python package to change their code. It may
   also highlight any additions, improvements, and bug fixes.

#. Type ``git status`` and check that you are on the ``master`` branch with no
   uncommitted code.

#. Mark the release with an empty commit, just to leave a marker. This is
   optional, but it makes it easier to find the release when skimming through
   the git history.

   .. code-block:: bash

      git commit --allow-empty -m "REL: vX.Y.Z"

#. Tag the commit.

   .. code-block:: bash

      git tag -a vX.Y.Z  # Don't forget the leading v

   This will create a tag named ``vX.Y.Z``. The ``-a`` flag (optional but
   recommended) opens up a text editor where you should enter a brief
   description of the release, such as "This releases fixes some bugs but does
   not introduce any breaking changes. All users are encouraged to upgrade."

#. Verify that the ``__version__`` attribute is correctly updated.

   The version is reported in three places:

   1. The git tag
   2. The version parameter in the ``setup.py`` file
   3. Your package's ``__version__`` attribute, in Python

   `Versioneer <https://github.com/warner/python-versioneer>`_, which was
   included and configured for you by the cookiecutter template, automatically
   keeps these three in sync. Just to be sure that it worked properly, start up
   Python, import the module, and check the ``__version__``.  It should have
   automatically updated to match the tag. The leading ``v`` is not included.

   .. code-block:: python

      import your_package
      your_package.__version__  # should be 'X.Y.Z'

#. Push the new commit and the tag to ``master``.

   .. code-block:: bash

      git push origin master
      git push origin vX.Y.Z

   .. note::

        Check your remotes using ``git remote -v``. If your respoitory is
        stored in an organization account, you may need to push to ``upstream``
        as well as ``origin``.

#. `Register for a PyPI account <https://pypi.org/account/register/>`_.

#. Install twine, a tool for uploading packages to PyPI.

   .. code-block:: bash

      python3 -m pip install --upgrade twine

#. Publish a release on PyPI.

   .. code-block:: bash

       python setup.py sdist
       python setup.py bdist_wheel
       twine upload dist/*

The package is now installable with pip. It may take a couple minutes to become
available.
