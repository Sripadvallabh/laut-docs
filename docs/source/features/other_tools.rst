Other tools
===========

Two commands ``query`` and ``info`` were added to LAUT to help users query dictionaries
and view information on genie parsers and apis. These are grouped under the section
'Other tools'.

``query``
---------

Applies a Dq query on a dictionary stored as a variable or parameter.

Simple example:

Using the api 'get_running_config_dict', the running config dictionary was stored
in the variable named 'runn'. To view 'ospf' related configuration, we can simply use
Dq shorthand 'ospf' to do so:

.. code-block:: console

   (lӓut-leaf1) query runn ospf
   {'interface GigabitEthernet1/0/1': {'ip ospf 10 area 0': {}},
    'interface Loopback0': {'ip ospf 10 area 0': {}},
    'router ospf 10': {}}
   (lӓut-leaf1)

This shows not only the ospf configuration, but also the interface names in which
ospf was configured.

To view a parameter dictionary prepend '%' to query argument as shown below:

.. code-block:: console

   (lӓut-leaf1) query %iol if
   {'if_name': 'gigabitEthernet1/0/1'}

Searching parameters is made easy with this command.

``info``
--------

View parser code by invoking ``info parser <SHOW_CMD>``.

View code for command 'show ip mroute' like this:

.. code-block:: console

   (lӓut-leaf1) info parser show ip mroute
   class ShowIpMrouteSchema(MetaParser):
       """Schema for:
           show ip mroute
           show ip mroute {group}
           show ip mroute {group} {source}
           show ip mroute verbose
           show ip mroute {group} verbose
           show ip mroute {group} {source} verbose
   <TRUNCATED>

``info parser`` always displays the parser schema as well.

View api code by invoking ``info api _<API_NAME>`` or ``info api <MODULE_NAME> <SUBMODULE_NAME> <API_NAME>``.

View code for api 'get_running_config_dict' like this:

.. code-block:: console

   (lӓut-leaf1) info api _get_running_config_dict
   def get_running_config_dict(device, option=None, output=None):
       """ Get show running-config output
   
           Args:
               device (`obj`): Device object
               option (`str`): option command
               output (`str`): output of show running-config
           Returns:
               config_dict (`dict`): dict of show run output
       """
       if output:
           out = output
       else:
           if option:
               cmd = "show running-config {}".format(option)
           else:
               cmd = "show running-config"
           try:
               out = device.execute(cmd)
           except SubCommandFailure as e:
               raise SubCommandFailure(
                   "Could not get running-config information "
                   "on device {device}".format(device=device.name)
               )
   
       config_dict = get_config_dict(out)
       return config_dict
   
   (lӓut-leaf1)

Invoking ``info`` command with '-o' argument opens the code in your editor
instead of printing on the terminal. This is much useful if you have syntax highlighting
in your editor.

.. code-block:: console

   (lӓut-leaf1) info api _get_running_config_dict -o
   (lӓut-leaf1)
