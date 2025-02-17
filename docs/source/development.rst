.. _developmenrt:

Development
===========

Codebase
--------

The pygeoapi codebase exists at https://github.com/geopython/pygeoapi.


Testing
-------

pygeoapi uses `pytest <https://docs.pytest.org>`_ for managing its automated tests.  Tests
exist in ``/tests`` and are developed for providers, formatters, processes, as well as the
overall API.

Tests can be run locally as part of development workflow.  They are also run on pygeoapi’s
`GitHub Actions setup`_ against all commits and pull requests to the code repository.

To run all tests, simply run ``pytest`` in the repository.  To run a specific test file,
run ``pytest tests/api/test_itemtypes.py``, for example.

Working with Spatialite on OSX
------------------------------

Using pyenv
^^^^^^^^^^^

It is common among OSX developers to use the package manager homebrew for the installation of pyenv to being able to manage multiple versions of Python.
They can encounter errors about the load of some SQLite extensions that pygeoapi uses for handling spatial data formats. In order to run properly the server
you are required to follow these steps below carefully.

Make Homebrew and pyenv play nicely together:

.. code-block:: bash

   # see https://github.com/pyenv/pyenv/issues/106
   alias brew='env PATH=${PATH//$(pyenv root)\/shims:/} brew'


Install Python with the option to enable SQLite extensions:

.. code-block:: bash

   LDFLAGS="-L/usr/local/opt/sqlite/lib -L/usr/local/opt/zlib/lib" CPPFLAGS="-I/usr/local/opt/sqlite/include -I/usr/local/opt/zlib/include" PYTHON_CONFIGURE_OPTS="--enable-loadable-sqlite-extensions" pyenv install 3.10.12

Configure SQLite from Homebrew over that one shipped with the OS:

.. code-block:: bash

   export PATH="/usr/local/opt/sqlite/bin:$PATH"

Install Spatialite from Homebrew:

.. code-block:: bash

   brew update
   brew install spatialite-tools
   brew libspatialite

Set the variable for the Spatialite library under OSX:

.. code-block:: bash

   SPATIALITE_LIBRARY_PATH=/usr/local/lib/mod_spatialite.dylib


.. _`GitHub Actions setup`: https://github.com/geopython/pygeoapi/blob/master/.github/workflows/main.yml


Using pre-commit
----------------

You may optionally use `pre-commit`_ in order to check for linting and other static issues
before committing changes. Pygeoapi's repo includes a ``.pre-commit.yml``
file, check the pre-commit docs on how to set it up - in a nutshell:

- pre-commit is mentioned in pygeoapi's ``requirements-dev.txt`` file, so it will be included
  when you pip install those
- run ``pre-commit install`` once in order to install its git commit hooks.
- optionally, run ``pre-commit run --all-files``, which will run all pre-commit hooks for all files in the repo.
  This also prepares the pre-commit environment.
- from now on, whenever you do a ``git commit``, the pre-commit hooks will run and the commit
  will only be done if all checks pass


.. _pre-commit:
