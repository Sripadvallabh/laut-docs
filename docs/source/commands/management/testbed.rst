
.. _testbed:

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
      leaf2-lag2:
        os: iosxe
        type: iosxe
        connections:
          cli:
            class: unicon.Unicon
            ip: 10.78.238.163
            port: 22
            protocol: ssh
          defaults:
            class: unicon.Unicon
            via: cli
        credentials:
          default:
            username: admin
            password: lab
      l2switch:
        os: iosxe
        type: iosxe
        connections:
          cli:
            class: unicon.Unicon
            ip: 10.78.238.164
            port: 22
            protocol: ssh
          defaults:
            class: unicon.Unicon
            via: cli
        credentials:
          default:
            username: admin
            password: lab

The application of ``testbed`` command to the above testbed YAML would look like this:

.. code-block:: console

    (lӓut-TB-NONE) testbed pyats/testbed.yaml
    2024-07-06 16:54:29: %LAUT-INFO: +------------------------------------------------------------------------------+
    2024-07-06 16:54:29: %LAUT-INFO: :                     Loading testbed 'pyats/testbed.yaml'                     :
    2024-07-06 16:54:29: %LAUT-INFO: +------------------------------------------------------------------------------+
    2024-07-06 16:54:29: %LAUT-INFO: +..............................................................................+
    2024-07-06 16:54:29: %LAUT-INFO: :                  Connecting to all devices in given testbed                  :
    2024-07-06 16:54:29: %LAUT-INFO: +..............................................................................+
    2024-07-06 16:54:29: %LAUT-INFO: +..............................................................................+
    2024-07-06 16:54:29: %LAUT-INFO: :                      Connecting to device 'leaf2-lag2'                       :
    2024-07-06 16:54:29: %LAUT-INFO: +..............................................................................+

    2024-07-06 16:54:29,189: %UNICON-INFO: +++ leaf2-lag2 logfile /tmp/leaf2-lag2-cli-20240706T165429189.log +++

    2024-07-06 16:54:29,190: %UNICON-INFO: +++ Unicon plugin iosxe (unicon.internal.plugins.iosxe) +++
    Warning: Permanently added '10.78.238.163' (RSA) to the list of known hosts.


    2024-07-06 16:54:29,523: %UNICON-INFO: +++ connection to spawn: ssh -l admin 10.78.238.163 -p 22 -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null, id: 139867410961648 +++

    2024-07-06 16:54:29,524: %UNICON-INFO: connection to leaf2-lag2
    Password:



    leaf2-lag2#

    leaf2-lag2#

    2024-07-06 16:54:34,135: %UNICON-INFO: +++ leaf2-lag2 with via 'cli': executing command 'show version | include operating mode' +++
    show version | include operating mode
    leaf2-lag2#

    2024-07-06 16:54:34,336: %UNICON-INFO: +++ initializing handle +++

    2024-07-06 16:54:34,459: %UNICON-INFO: +++ leaf2-lag2 with via 'cli': executing command 'term length 0' +++
    term length 0
    leaf2-lag2#
    2024-07-06 16:54:34: %LAUT-INFO: +..............................................................................+
    2024-07-06 16:54:34: %LAUT-INFO: :                       Connecting to device 'l2switch'                        :
    2024-07-06 16:54:34: %LAUT-INFO: +..............................................................................+

    2024-07-06 16:54:34,744: %UNICON-INFO: +++ l2switch logfile /tmp/l2switch-cli-20240706T165434743.log +++

    2024-07-06 16:54:34,744: %UNICON-INFO: +++ Unicon plugin iosxe (unicon.internal.plugins.iosxe) +++
    Warning: Permanently added '10.78.238.164' (RSA) to the list of known hosts.


    2024-07-06 16:54:35,083: %UNICON-INFO: +++ connection to spawn: ssh -l admin 10.78.238.164 -p 22 -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null, id: 139867411030752 +++

    2024-07-06 16:54:35,083: %UNICON-INFO: connection to l2switch
    Password:



    l2switch#

    l2switch#

    2024-07-06 16:54:39,631: %UNICON-INFO: +++ l2switch with via 'cli': executing command 'show version | include operating mode' +++
    show version | include operating mode
    l2switch#

    2024-07-06 16:54:39,821: %UNICON-INFO: +++ initializing handle +++

    2024-07-06 16:54:39,944: %UNICON-INFO: +++ l2switch with via 'cli': executing command 'term length 0' +++
    term length 0
    l2switch#
    2024-07-06 16:54:40: %LAUT-INFO: +------------------------------------------------------------------------------+
    2024-07-06 16:54:40: %LAUT-INFO: :                           Testbed load successful                            :
    2024-07-06 16:54:40: %LAUT-INFO: +------------------------------------------------------------------------------+
    (lӓut-leaf2-lag2)


For more information on testbed YAML, refer the detailed explanation of a pyATS testbed
YAML schema `here <https://pubhub.devnetcloud.com/media/pyats/docs/topology/schema.html#production-yaml-schema>`_

.. note::

   At startup, there won't be any testbed loaded onto LAUT & hence no devices actively
   loaded onto the shell; this is represented via the prompt ``(lӓut-TB-NONE)``.
