Operator commands
====================

These commands *operate* on device to acheive a certain task
representing the basic blitz actions and are primarily used to autogenerate
these blitz actions snippets. Apart from ``execute`` and ``parse`` which autogenerate their
blitz action snippets under certain criteria, rest all other commands always autogenerate
their corresponding blitz action snippets.

* :doc:`execute` - Execute commands; can autogenerate *'execute'* blitz action
* :doc:`configure` - Configure CLI; always autogenerates *'configure'* blitz action
* :doc:`parse` - Parse commands; can autogenerate *'parse'* blitz action
* :doc:`api` - Invoke Genie API's; always autogenerates *'api'* blitz action
* :doc:`replay` - Run generated blitz testcases; always autogenerates *'run_genie_sdk'* blitz action
* :doc:`sleep` - Sleep; always autogenerates *'sleep'* blitz action

.. toctree::
   :maxdepth: 1
   :hidden:

   execute
   configure
   parse
   api
   replay
   sleep
