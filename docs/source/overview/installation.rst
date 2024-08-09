Installation Instructions
=========================

Step 1: Pull live-aut repository

.. code-block:: console

   git clone git@polaris-git.cisco.com:bashish/live-aut.git

Step 2: Source pyats environment

Step 3: Install dependencies via pip in pyats env

.. code-block:: console

   pip install cmd2 ruamel.yaml pathlib colorama datetime jinja2 gnureadline

Step 4: Start LAUT

.. code-block:: console

   cd live-aut
   python3 laut.py
