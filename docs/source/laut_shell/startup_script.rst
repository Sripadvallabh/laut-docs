Startup script
==============

LAUT uses a startup script located at '~/.lautrc' to invoke certain LAUT commands at startup.
This can be used to permanently store alias, macros and settings and might as well load testbeds,
import parameters and more.

Usage
-----

First, create a file '.lautrc' in your home directory and add the LAUT commands
that are needed at startup to it. An example is shown here:

.. code-block:: text

   alias create debug execute debug
   macro create route show ip route {1}
   testbed ../../alternate_msdp/testbed.yaml

LAUT automatically fetches '~/.lautrc' and runs all the commands in at startup as seen below:

.. code-block:: console

   $ /ws/araradha-bgl/laut
   
   🎃 LAUT shell started
   
   Alias 'debug' created
   Macro 'route' created
   2024-07-31 16:22:00: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-31 16:22:00: %LAUT-INFO: :             Loading testbed '../../alternate_msdp/testbed.yaml'              :
   2024-07-31 16:22:00: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-31 16:22:01: %LAUT-INFO: +..............................................................................+
   2024-07-31 16:22:01: %LAUT-INFO: :                  Connecting to all devices in given testbed                  :
   2024-07-31 16:22:01: %LAUT-INFO: +..............................................................................+
   2024-07-31 16:22:01: %LAUT-INFO: +..............................................................................+
   2024-07-31 16:22:01: %LAUT-INFO: :                         Connecting to device 'host1'                         :
   2024-07-31 16:22:01: %LAUT-INFO: +..............................................................................+
   
   2024-07-31 16:22:01,626: %UNICON-INFO: +++ host1 logfile /tmp/host1-cli-20240731T162201625.log +++
   
   2024-07-31 16:22:01,626: %UNICON-INFO: +++ Unicon plugin iosxe (unicon.internal.plugins.iosxe) +++
   /nobackup/araradha/pyats/lib/python3.8/site-packages/unicon/bases/routers/connection.py:97: DeprecationWarning: Arguments 'username', 'enable_password','tacacs_password' and 'line_password' are now deprecated and replaced by 'credentials'.
     warnings.warn(message = "Arguments 'username', "
   Trying 127.0.0.1...
   Connected to 127.0.0.1.
   Escape character is '^]'.
   
   
   2024-07-31 16:22:01,656: %UNICON-INFO: +++ connection to spawn: telnet 127.0.0.1 33331, id: 140103132583440 +++
   
   2024-07-31 16:22:01,657: %UNICON-INFO: connection to host1
   
   host1#
   
   host1#
   
   2024-07-31 16:22:02,665: %UNICON-INFO: +++ host1 with via 'a': executing command 'show version | include operating mode' +++
   show version | include operating mode
   host1#
   
   2024-07-31 16:22:03,026: %UNICON-INFO: +++ initializing handle +++
   
   2024-07-31 16:22:03,162: %UNICON-INFO: +++ host1 with via 'a': executing command 'term length 0' +++
   term length 0
   host1#
   2024-07-31 16:22:03: %LAUT-INFO: +..............................................................................+
   2024-07-31 16:22:03: %LAUT-INFO: :                         Connecting to device 'host2'                         :
   2024-07-31 16:22:03: %LAUT-INFO: +..............................................................................+
   
   2024-07-31 16:22:03,416: %UNICON-INFO: +++ host2 logfile /tmp/host2-cli-20240731T162203415.log +++
   
   2024-07-31 16:22:03,416: %UNICON-INFO: +++ Unicon plugin iosxe (unicon.internal.plugins.iosxe) +++
   Trying 127.0.0.1...
   
   
   2024-07-31 16:22:03,430: %UNICON-INFO: +++ connection to spawn: telnet 127.0.0.1 33332, id: 140103130214304 +++
   
   2024-07-31 16:22:03,430: %UNICON-INFO: connection to host2
   Connected to 127.0.0.1.
   Escape character is '^]'.
   
   host2#
   
   host2#
   
   2024-07-31 16:22:04,393: %UNICON-INFO: +++ host2 with via 'a': executing command 'show version | include operating mode' +++
   show version | include operating mode
   host2#
   
   2024-07-31 16:22:04,556: %UNICON-INFO: +++ initializing handle +++
   
   2024-07-31 16:22:04,679: %UNICON-INFO: +++ host2 with via 'a': executing command 'term length 0' +++
   term length 0
   host2#
   2024-07-31 16:22:04: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-31 16:22:04: %LAUT-INFO: :                           Testbed load successful                            :
   2024-07-31 16:22:04: %LAUT-INFO: +------------------------------------------------------------------------------+
   (lӓut-host1)

Note the alias & macro getting initialized(this helps to always keep aliases & macros in all LAUT sessions)
along with the loading of a testbed; all from the commands mentioned in the startup script.
