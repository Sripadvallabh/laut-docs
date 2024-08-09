Info messages
=============

For every action performed on LAUT shell, LAUT-INFO & LAUT-ERR messages will be printed for
the user.

LAUT-INFO
---------

LAUT-INFO tells information about the state of the action and is printed along with the date
and time of execution.

An example of LAUT-INFO message for 'api' parameters:

.. code-block:: console

   2024-08-01 09:45:30: %LAUT-INFO: +..............................................................................+
   2024-08-01 09:45:30: %LAUT-INFO: :                Api 'get_running_config_dict' with parameters:                :
   2024-08-01 09:45:30: %LAUT-INFO: :                                device: host1                                 :
   2024-08-01 09:45:30: %LAUT-INFO: :                                 option: None                                 :
   2024-08-01 09:45:30: %LAUT-INFO: :                                 output: None                                 :
   2024-08-01 09:45:30: %LAUT-INFO: +..............................................................................+

LAUT-ERR
---------

When anything goes wrong, LAUT-ERR will be printed in 'RED' along with the reason for the failure. Following are some
examples:

Testbed already loaded:-

.. code-block:: console

   (lӓut-host1) testbed ../../alternate_msdp/testbed.yaml
   2024-08-01 09:47:12: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 09:47:12: %LAUT-INFO: :             Loading testbed '../../alternate_msdp/testbed.yaml'              :
   2024-08-01 09:47:12: %LAUT-INFO: +------------------------------------------------------------------------------+
   LAUT-ERR: Testbed '../../alternate_msdp/testbed.yaml' already loaded in shell
   (lӓut-host1)

Failed ``replay``:

.. code-block:: console

   (lӓut-host1) replay tc.yaml
   2024-08-01 09:48:11: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 09:48:11: %LAUT-INFO: :                         Starting run_genie_sdk "tc"                          :
   2024-08-01 09:48:11: %LAUT-INFO: +------------------------------------------------------------------------------+
   <TRUNCATED>
   2024-08-01 09:48:11: %LAUT-INFO: +..............................................................................+
   2024-08-01 09:48:11: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-08-01 09:48:11: %LAUT-INFO: +..............................................................................+
   2024-08-01 09:48:11: %LAUT-ERR : :                        '4.4.4.4' doesn't match the output                    :
   2024-08-01 09:48:11: %LAUT-INFO: +..............................................................................+
   <TRUNCATED>

Optionally in some scenarios, a detailed error message will be printed in 'YELLOW' other than the LAUT-ERR message.
In the below example, LAUT-ERR mentiones the failure to execute, but the message in YELLOW below shows the exact reason
as a sub command failure from pyATS API.

.. code-block:: console

   (lӓut-host1) show sfasdf
   2024-08-01 09:50:08: %LAUT-INFO: +..............................................................................+
   2024-08-01 09:50:08: %LAUT-INFO: :                       Execute 'show sfasdf' on 'host1'                       :
   2024-08-01 09:50:08: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 09:50:08,380: %UNICON-INFO: +++ host1 with via 'a': executing command 'show sfasdf' +++
   show sfasdf
   show sfasdf
         ^
   % Invalid input detected at '^' marker.
   
   host1#
   LAUT-ERR: Command 'show sfasdf' failed to execute on 'host1'
   ('Command execution failed', SubCommandFailure('sub_command failure, patterns matched in the output:', ['^%\\s*[Ii]nvalid (command|input)'], 'service result', "show sfasdf\r\nshow sfasdf\r\n      ^\r\n% Invalid input detected at '^' marker.\r\n\r\nhost1#"))
   (lӓut-host1)

.. note::

   When a LAUT-ERR message is printed for any command, that command will fail to autogenerate the blitz
   action snippet in case it is intended to autogenerate one.
