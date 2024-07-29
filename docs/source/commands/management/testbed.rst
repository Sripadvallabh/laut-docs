testbed
=======

Loads a particular testbed onto LAUT shell.

LAUT needs to work with network devices & needs information regarding the same.
This information needs to be provided via a pyATS testbed YAML. The ``testbed`` command
takes the filepath to the testbed YAML as an argument. Once a testbed YAML is provided,
LAUT connects to each & every device mentioned in the testbed file via pyATS ``connect()`` API
& displays the '*Testbed load successful*' INFO message when all devices are successfully
connected.

For example, given 2 IOS-XE devices in a testbed YAML as follows:

.. code-block:: yaml

   devices:
     host1:
       os: iosxe
       type: iosxe
       platform: iol
       connections:
         a:
           ip: 127.0.0.1
           port: 33331
           protocol: telnet
         defaults:
           class: unicon.Unicon
           via: cli
     host2:
       os: iosxe
       type: iosxe
       platform: iol
       connections:
         a:
           ip: 127.0.0.1
           port: 33332
           protocol: telnet
         defaults:
           class: unicon.Unicon
           via: cli

The application of ``testbed`` command to the above testbed YAML would look like this:

.. code-block:: console

   (lӓut-TB-NONE) testbed pyats/testbed.yaml
   2024-07-26 14:04:30: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-26 14:04:30: %LAUT-INFO: :                     Loading testbed 'pyats/testbed.yaml'                     :
   2024-07-26 14:04:30: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-26 14:04:35: %LAUT-INFO: +..............................................................................+
   2024-07-26 14:04:35: %LAUT-INFO: :                  Connecting to all devices in given testbed                  :
   2024-07-26 14:04:35: %LAUT-INFO: +..............................................................................+
   2024-07-26 14:04:35: %LAUT-INFO: +..............................................................................+
   2024-07-26 14:04:35: %LAUT-INFO: :                         Connecting to device 'host1'                         :
   2024-07-26 14:04:35: %LAUT-INFO: +..............................................................................+
   
   2024-07-26 14:04:35,131: %UNICON-INFO: +++ host1 logfile /tmp/host1-cli-20240726T140435130.log +++
   
   2024-07-26 14:04:35,131: %UNICON-INFO: +++ Unicon plugin iosxe (unicon.internal.plugins.iosxe) +++
   /nobackup/araradha/pyats/lib/python3.8/site-packages/unicon/bases/routers/connection.py:97: DeprecationWarning: Arguments 'username', 'enable_password','tacacs_password' and 'line_password' are now deprecated and replaced by 'credentials'.
     warnings.warn(message = "Arguments 'username', "
   Trying 127.0.0.1...
   Connected to 127.0.0.1.
   Escape character is '^]'.
   
   
   2024-07-26 14:04:35,176: %UNICON-INFO: +++ connection to spawn: telnet 127.0.0.1 33331, id: 140026962501840 +++
   
   2024-07-26 14:04:35,176: %UNICON-INFO: connection to host1
   
   host1#
   
   host1#
   
   2024-07-26 14:04:36,360: %UNICON-INFO: +++ host1 with via 'a': executing command 'show version | include operating mode' +++
   show version | include operating mode
   host1#
   
   2024-07-26 14:04:36,540: %UNICON-INFO: +++ initializing handle +++
   
   2024-07-26 14:04:36,692: %UNICON-INFO: +++ host1 with via 'a': executing command 'term length 0' +++
   term length 0
   host1#
   2024-07-26 14:04:36: %LAUT-INFO: +..............................................................................+
   2024-07-26 14:04:36: %LAUT-INFO: :                         Connecting to device 'host2'                         :
   2024-07-26 14:04:36: %LAUT-INFO: +..............................................................................+
   
   2024-07-26 14:04:36,999: %UNICON-INFO: +++ host2 logfile /tmp/host2-cli-20240726T140436997.log +++
   
   2024-07-26 14:04:36,999: %UNICON-INFO: +++ Unicon plugin iosxe (unicon.internal.plugins.iosxe) +++
   Trying 127.0.0.1...
   Connected to 127.0.0.1.
   Escape character is '^]'.
   
   
   2024-07-26 14:04:37,058: %UNICON-INFO: +++ connection to spawn: telnet 127.0.0.1 33332, id: 140026959604368 +++
   
   2024-07-26 14:04:37,058: %UNICON-INFO: connection to host2
   
   host2#
   
   host2#
   
   2024-07-26 14:04:38,122: %UNICON-INFO: +++ host2 with via 'a': executing command 'show version | include operating mode' +++
   show version | include operating mode
   host2#
   
   2024-07-26 14:04:38,269: %UNICON-INFO: +++ initializing handle +++
   
   2024-07-26 14:04:38,445: %UNICON-INFO: +++ host2 with via 'a': executing command 'term length 0' +++
   term length 0
   host2#
   2024-07-26 14:04:38: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-26 14:04:38: %LAUT-INFO: :                           Testbed load successful                            :
   2024-07-26 14:04:38: %LAUT-INFO: +------------------------------------------------------------------------------+
   (lӓut-host1)

For more information on testbed YAML, refer the detailed explanation of a pyATS testbed
YAML schema `here <https://pubhub.devnetcloud.com/media/pyats/docs/topology/schema.html#production-yaml-schema>`_

.. note::

   At startup, there won't be any testbed loaded onto LAUT & hence no devices actively
   loaded onto the shell; this is represented via the prompt ``(lӓut-TB-NONE)``.
