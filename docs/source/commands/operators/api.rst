api
===

Invokes Genie API on the device loaded in shell and autogenerates pyATS
blitz *'api'* action snippet.

.. note::

   If you are new to Genie API's, please refer to Genie's `api documentation <https://wwwin-enged.cisco.com/elearning/coursesp/pyats/user/apis.html>`_.

Why api
-------

Abstracts & modularizes repetitive basic tasks. And with code written in python, can handle higher
level tasks not possible with manual devices or via LAUT commands.

Basic usage
-----------

With LAUT, users can invoke any genie API in 2 ways:
   1. ``api <MODULE_NAME> <SUBMODULE_NAME> <API_NAME>``: Use module names to navigate to the API(Autocompletion helps much here)
   2. ``api _<API_NAME>``: Prepend a '_' before <API_NAME> to directly invoke an API by its name

A simple example of an api ``get_interface_names`` called on device ``host2`` listing all interface
names.

.. code-block:: console

   (lӓut-host1) api _get_interface_names
   Gets the names of all interfaces on the device
   
   Args:
       device (obj): Device object
   
   Returns:
       list: List of interface names
   
   device: [<Device host1 at 0x7f1b4d2d40a0>]
   2024-07-28 11:19:28: %LAUT-INFO: +..............................................................................+
   2024-07-28 11:19:28: %LAUT-INFO: :                  Api 'get_interface_names' with parameters:                  :
   2024-07-28 11:19:28: %LAUT-INFO: :                                device: host1                                 :
   2024-07-28 11:19:28: %LAUT-INFO: +..............................................................................+
   
   2024-07-28 11:19:29,011: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ip interface brief' +++
   show ip interface brief
   Interface              IP-Address      OK? Method Status                Protocol
   Ethernet0/0            10.10.10.1      YES NVRAM  administratively down down
   Ethernet0/1            unassigned      YES NVRAM  administratively down down
   Ethernet0/2            unassigned      YES NVRAM  administratively down down
   Ethernet0/3            unassigned      YES NVRAM  administratively down down
   Ethernet1/0            unassigned      YES NVRAM  administratively down down
   Ethernet1/1            unassigned      YES NVRAM  administratively down down
   Ethernet1/2            unassigned      YES NVRAM  administratively down down
   Ethernet1/3            unassigned      YES NVRAM  administratively down down
   Ethernet2/0            unassigned      YES NVRAM  administratively down down
   Ethernet2/1            unassigned      YES NVRAM  administratively down down
   Ethernet2/2            unassigned      YES NVRAM  administratively down down
   Ethernet2/3            unassigned      YES NVRAM  administratively down down
   Ethernet3/0            unassigned      YES NVRAM  administratively down down
   Ethernet3/1            unassigned      YES NVRAM  administratively down down
   Ethernet3/2            unassigned      YES NVRAM  administratively down down
   Ethernet3/3            unassigned      YES NVRAM  administratively down down
   Serial4/0              unassigned      YES NVRAM  administratively down down
   Serial4/1              unassigned      YES NVRAM  administratively down down
   Serial4/2              unassigned      YES NVRAM  administratively down down
   Serial4/3              unassigned      YES NVRAM  administratively down down
   Serial5/0              unassigned      YES NVRAM  administratively down down
   Serial5/1              unassigned      YES NVRAM  administratively down down
   Serial5/2              unassigned      YES NVRAM  administratively down down
   Serial5/3              unassigned      YES NVRAM  administratively down down
   Loopback0              1.1.1.1         YES NVRAM  administratively down down
   Loopback1              unassigned      YES unset  up                    up
   Loopback30             1.1.1.1         YES manual up                    up
   Loopback99             unassigned      YES manual up                    up
   Tunnel0                unassigned      YES unset  up                    down
   Tunnel1                unassigned      YES unset  up                    down
   Tunnel2                unassigned      YES unset  up                    down
   Tunnel3                unassigned      YES unset  up                    down
   Tunnel4                unassigned      YES unset  up                    down
   host1#
   2024-07-28 11:19:29: %LAUT-INFO: +..............................................................................+
   2024-07-28 11:19:29: %LAUT-INFO: :                                 Api output:                                  :
   2024-07-28 11:19:29: %LAUT-INFO: +..............................................................................+
                                                                     ['Ethernet0/0'
                                                                     'Ethernet0/1'
                                                                     'Ethernet0/2'
                                                                     'Ethernet0/3'
                                                                     'Ethernet1/0'
                                                                     'Ethernet1/1'
                                                                     'Ethernet1/2'
                                                                     'Ethernet1/3'
                                                                     'Ethernet2/0'
                                                                     'Ethernet2/1'
                                                                     'Ethernet2/2'
                                                                     'Ethernet2/3'
                                                                     'Ethernet3/0'
                                                                     'Ethernet3/1'
                                                                     'Ethernet3/2'
                                                                     'Ethernet3/3'
                                                                      'Serial4/0'
                                                                      'Serial4/1'
                                                                      'Serial4/2'
                                                                      'Serial4/3'
                                                                      'Serial5/0'
                                                                      'Serial5/1'
                                                                      'Serial5/2'
                                                                      'Serial5/3'
                                                                      'Loopback0'
                                                                      'Loopback1'
                                                                      'Loopback30'
                                                                      'Loopback99'
                                                                       'Tunnel0'
                                                                       'Tunnel1'
                                                                       'Tunnel2'
                                                                       'Tunnel3'
                                                                       'Tunnel4']
   2024-07-28 11:19:29: %LAUT-INFO: +..............................................................................+
   (lӓut-host1)

Invoking an ``api`` command always
