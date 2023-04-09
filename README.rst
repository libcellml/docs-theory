Tutorials for libCellML
=======================

A collection of tutorials demonstrating in context how libCellML can be used to create, manipulate, and solve CellML models.

Running locally
---------------

To see the prepared documentation in a local browser when editing the documentation follow these steps:
(Assuming that the tutorial source has been cloned to a directory named *tutorials*.)

1. Create a virtual environment::

    python -m venv venv-sphinx

2. Activate and install required packages::

    source venv-sphinx/bin/Activate
    cd tutorials
    pip install -r requirements.txt

3. Build documentation::

    sphinx-build . build

4. Serve documentation::

    python -m http.server

5. View locally built documentation by visiting::

    http://localhost:8000
