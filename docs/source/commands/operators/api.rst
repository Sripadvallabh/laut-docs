api
===

Invokes Genie API on the device loaded in shell and autogenerates pyATS
blitz *'api'* action snippet.

.. note::

   If you are new to Genie API's, please refer to Genie's `api documentation <https://wwwin-enged.cisco.com/elearning/coursesp/pyats/user/apis.html>`_.

Why api
-------

Genie API's can abstract complex tasks because with code written in python, it can handle
those tasks which are not possible with LAUT commands.

Api invoke methods
------------------

With LAUT, users can invoke any genie API in 2 ways:
   1. ``api <MODULE_NAME> <SUBMODULE_NAME> <API_NAME>``: Use module names to navigate to the API
   2. ``api _<API_NAME>``: Prepend a '_' before <API_NAME> to directly invoke an API by its name

No matter which way the api was invoked, it is finally the same genie API code that gets executed. The first
method provides users with sorting out what API's are present under each module to better understand them. The 
submodule names are usually of the format 'get', 'configure', 'verify' ... which are essentially categorizing
the functionality of the API towards the devices. As an example, to find configuration related API's for vrfs,
one can search with the command ``api vrf configure <TAB><TAB>``:

.. code-block:: console

   (lӓut-host2) api vrf configure
   configure_default_mpls_mldp                       configure_vrf_definition_family
   configure_default_vxlan                           configure_vrf_definition_stitching
   configure_ip_vrf_forwarding_interface             configure_vrf_description
   configure_mdt_auto_discovery_inter_as             configure_vrf_forwarding_interface
   configure_mdt_auto_discovery_inter_as_mdt_type    configure_vrf_rd_value
   configure_mdt_auto_discovery_mldp                 create_ip_vrf
   configure_mdt_auto_discovery_vxlan                delete_ip_vrf
   configure_mdt_data_mpls_mldp                      unconfigure_default_vxlan
   configure_mdt_data_threshold                      unconfigure_ip_vrf_forwarding_interface
   configure_mdt_data_vxlan                          unconfigure_mdt_auto_discovery_mldp
   configure_mdt_default                             unconfigure_mdt_auto_discovery_vxlan
   configure_mdt_overlay_use_bgp                     unconfigure_mdt_data_threshold
   configure_mdt_overlay_use_bgp_spt_only            unconfigure_mdt_data_vxlan
   configure_mdt_partitioned_mldp_p2mp               unconfigure_mdt_overlay_use_bgp
   configure_mdt_preference_under_vrf                unconfigure_vrf
   configure_mdt_strict_rpf_interface_vrf            unconfigure_vrf_definition_on_device
   configure_multicast_routing_mvpn_vrf              unconfigure_vrf_definition_stitching
   configure_rd_address_family_vrf                   unconfigure_vrf_description
   configure_scale_vrf_via_tftp                      unconfigure_vrf_forwarding_interface
   configure_vpn_id_in_vrf

However, if the API name is already known (or) the search for a particular API was done via `Genie API browser <https://pubhub.devnetcloud.com/media/genie-feature-browser/docs/#/apis>`_, one can directly invoke the API via the 2nd format mentioned above.

Basic usage
-----------

A simple example of an *get* api *'get_interface_mtu_size'* called on device 'host2' returning the MTU size
for a given interface(Since the API name is known, the command invoked is ``api _get_interface_mtu_size``):

.. code-block:: console

   (lӓut-host2) api _get_interface_mtu_size
   Get interface MTU
   
   Args:
       device (`obj`): Device object
       interface (`str`): Interface name
   
   Returns:
       None
       mtu (`int`): mtu bytes
   
   Raises:
       None
   
   device: [<Device host2 at 0x7f011f2e4a30>]
   interface: Loopback0
   2024-07-30 10:53:49: %LAUT-INFO: +..............................................................................+
   2024-07-30 10:53:49: %LAUT-INFO: :                Api 'get_interface_mtu_size' with parameters:                 :
   2024-07-30 10:53:49: %LAUT-INFO: :                                device: host2                                 :
   2024-07-30 10:53:49: %LAUT-INFO: :                            interface: 'Loopback0'                            :
   2024-07-30 10:53:49: %LAUT-INFO: +..............................................................................+
   
   2024-07-30 10:53:50,050: %UNICON-INFO: +++ host2 with via 'a': executing command 'show interfaces Loopback0' +++
   show interfaces Loopback0
   Loopback0 is administratively down, line protocol is down
     Hardware is Loopback
     Internet address is 1.1.1.1/32
     MTU 1514 bytes, BW 8000000 Kbit/sec, DLY 5000 usec,
        reliability 255/255, txload 1/255, rxload 1/255
     Encapsulation LOOPBACK, loopback not set
     Keepalive set (10 sec)
     Last input never, output never, output hang never
     Last clearing of "show interface" counters never
     Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
     Queueing strategy: fifo
     Output queue: 0/0 (size/max)
     5 minute input rate 0 bits/sec, 0 packets/sec
     5 minute output rate 0 bits/sec, 0 packets/sec
        0 packets input, 0 bytes, 0 no buffer
        Received 0 broadcasts (0 IP multicasts)
        0 runts, 0 giants, 0 throttles
        0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
        0 packets output, 0 bytes, 0 underruns
        Output 0 broadcasts (0 IP multicasts)
        0 output errors, 0 collisions, 0 interface resets
        0 unknown protocol drops
        0 output buffer failures, 0 output buffers swapped out
   host2#
   2024-07-30 10:53:50: %LAUT-INFO: +..............................................................................+
   2024-07-30 10:53:50: %LAUT-INFO: :                                 Api output:                                  :
   2024-07-30 10:53:50: %LAUT-INFO: +..............................................................................+
                                                                          1514
   2024-07-30 10:53:50: %LAUT-INFO: +..............................................................................+
   (lӓut-host2)

Invoking an ``api`` command always prints the docstring of the API for user's reference as highlighted below
from the earlier example:

.. code-block:: console

   (lӓut-host2) api _get_interface_mtu_size
   Get interface MTU
   
   Args:
       device (`obj`): Device object
       interface (`str`): Interface name
   
   Returns:
       None
       mtu (`int`): mtu bytes
   
   Raises:
       None
   
After the docstring is printed, the arguments are prompted from the user one by one. The 'device' argument
is not prompted from the user and will be the device loaded in LAUT shell. As for the rest of the parameters,
the user needs to type each of them one by one. Note that the arguments are literal evaluvated using AST by LAUT to
determine their actual value(i.e. passing an argument of 20 would mean the integer 20 whereas passing '20' would mean
the string '20'). Default parameter values are shown in parenthesis after their
names, so a newline from the user would indicate the default value to be used by LAUT. See below example to understand
a bit more:

.. code-block:: console

   (lӓut-host2) api _get_running_config_dict
   Get show running-config output
   
   Args:
       device (`obj`): Device object
       option (`str`): option command
       output (`str`): output of show running-config
   Returns:
       config_dict (`dict`): dict of show run output
   
   device: [<Device host2 at 0x7f011f2e4a30>]
   option(None):
   output(None):
   2024-07-30 10:59:39: %LAUT-INFO: +..............................................................................+
   2024-07-30 10:59:39: %LAUT-INFO: :                Api 'get_running_config_dict' with parameters:                :
   2024-07-30 10:59:39: %LAUT-INFO: :                                device: host2                                 :
   2024-07-30 10:59:39: %LAUT-INFO: :                                 option: None                                 :
   2024-07-30 10:59:39: %LAUT-INFO: :                                 output: None                                 :
   2024-07-30 10:59:39: %LAUT-INFO: +..............................................................................+

The API function definition for reference below:

.. code-block:: python

   def get_running_config_dict(device, option=None, output=None):

As seen above, 3 parameters were required for the api 'get_running_config_dict'. The device parameter was already
printed by LAUT to be 'host2' which was loaded in the shell; and the other two parameters 'option' and 'output' were
having a default of 'None'. To allow LAUT to pass default values for the other two parameters, a newline feed was
sufficient. 

For every API invoked, LAUT always prints 'LAUT-INFO' messages indicating the name of api as well as its parameters
passed as seen above. This is useful to understand what value was passed for each parameter after LAUT does its literal
evaluvation. From the 'LAUT-INFO' messages printed for the above example, we could infer that indeed the
'option' and 'output' parameters were passed None.

Include entries
---------------

To add an *include* entry, invoke ``api`` command with the argument '-i' which would prompt the user for adding
the *include* entries. For API, *include* entries vary with the type of output an API returns.

.. list-table:: Include entry type w.r.t API output type
   :widths: 50 50
   :header-rows: 1

   * - API output type
     - Include entry type
   * - str
     - Exact string seen in output
   * - dict
     - Dq query, Line numbers
   * - int
     - Mathematical expression {==,>=,<=,<,>,!=}
   * - list
     - List values
   * - bool
     - True/False
   * - None
     - NOT PERMITTED

**str**

Most of the api's return strings, only the entire string can be added as an *include* entry; regex
patterns are not allowed. See a simple example below:

.. code-block:: console

   (lӓut-host1) api _get_interface_ip_address -i
   Get interface ip_address from device
   
   Args:
       interface('str'): Interface to get address
       device ('obj'): Device object
       address_family ('str'): Used only for junos api
   
   Returns:
       None
       interface ip_address ('str')
   
   Raises:
       None
   
   device: [<Device host1 at 0x7f4b88a53370>]
   interface: Loopback0
   address_family(None):
   2024-08-08 07:55:22: %LAUT-INFO: +..............................................................................+
   2024-08-08 07:55:22: %LAUT-INFO: :               Api 'get_interface_ip_address' with parameters:                :
   2024-08-08 07:55:22: %LAUT-INFO: :                                device: host1                                 :
   2024-08-08 07:55:22: %LAUT-INFO: :                            interface: 'Loopback0'                            :
   2024-08-08 07:55:22: %LAUT-INFO: :                             address_family: None                             :
   2024-08-08 07:55:22: %LAUT-INFO: +..............................................................................+
   
   2024-08-08 07:55:23,141: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ip interface brief Loopback0' +++
   show ip interface brief Loopback0
   Interface              IP-Address      OK? Method Status                Protocol
   Loopback0              1.1.1.1         YES NVRAM  up                    up
   host1#
   2024-08-08 07:55:23: %LAUT-INFO: +..............................................................................+
   2024-08-08 07:55:23: %LAUT-INFO: :                                 Api output:                                  :
   2024-08-08 07:55:23: %LAUT-INFO: +..............................................................................+
                                                                       '1.1.1.1'
   2024-08-08 07:55:23: %LAUT-INFO: +..............................................................................+
   2024-08-08 07:55:23: %LAUT-INFO: +..............................................................................+
   2024-08-08 07:55:23: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-08-08 07:55:23: %LAUT-INFO: +..............................................................................+
   Enter pattern to INCLUDE (Press enter for multiple patterns): 1.1.1.1
   (lӓut-host1) list 1
   api:
     function: get_interface_ip_address
     arguments:
       device: host1
       interface: Loopback0
       address_family:
     include:
       - 1.1.1.1

**dict**

*Include* entries for type 'dict' are similar to ``parse``; see :doc:`parse` for more information.

**int**

An example for 'int' *include* entry is shown below where we check if MTU size is within the range 1000 & 2000:

.. code-block:: console

   (lӓut-host1) api _get_interfacemtu
   Hint:
     name  Api name/path
   
   (lӓut-host1) api _get_interface_mtu_size -i
   Get interface MTU
   
   Args:
       device (`obj`): Device object
       interface (`str`): Interface name
   
   Returns:
       None
       mtu (`int`): mtu bytes
   
   Raises:
       None
   
   device: [<Device host1 at 0x7f4b88a53370>]
   interface: Loopback0
   2024-08-08 07:56:54: %LAUT-INFO: +..............................................................................+
   2024-08-08 07:56:54: %LAUT-INFO: :                Api 'get_interface_mtu_size' with parameters:                 :
   2024-08-08 07:56:54: %LAUT-INFO: :                                device: host1                                 :
   2024-08-08 07:56:54: %LAUT-INFO: :                            interface: 'Loopback0'                            :
   2024-08-08 07:56:54: %LAUT-INFO: +..............................................................................+
   
   2024-08-08 07:56:54,682: %UNICON-INFO: +++ host1 with via 'a': executing command 'show interfaces Loopback0' +++
   show interfaces Loopback0
   Loopback0 is up, line protocol is up
     Hardware is Loopback
     Internet address is 1.1.1.1/32
     MTU 1514 bytes, BW 8000000 Kbit/sec, DLY 5000 usec,
        reliability 255/255, txload 1/255, rxload 1/255
     Encapsulation LOOPBACK, loopback not set
     Keepalive set (10 sec)
     Last input 00:00:02, output never, output hang never
     Last clearing of "show interface" counters never
     Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
     Queueing strategy: fifo
     Output queue: 0/0 (size/max)
     5 minute input rate 0 bits/sec, 0 packets/sec
     5 minute output rate 0 bits/sec, 0 packets/sec
        0 packets input, 0 bytes, 0 no buffer
        Received 0 broadcasts (0 IP multicasts)
        0 runts, 0 giants, 0 throttles
        0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
        56221 packets output, 2535652 bytes, 0 underruns
        Output 0 broadcasts (0 IP multicasts)
        0 output errors, 0 collisions, 0 interface resets
        0 unknown protocol drops
        0 output buffer failures, 0 output buffers swapped out
   host1#
   2024-08-08 07:56:54: %LAUT-INFO: +..............................................................................+
   2024-08-08 07:56:54: %LAUT-INFO: :                                 Api output:                                  :
   2024-08-08 07:56:54: %LAUT-INFO: +..............................................................................+
                                                                          1514
   2024-08-08 07:56:54: %LAUT-INFO: +..............................................................................+
   2024-08-08 07:56:54: %LAUT-INFO: +..............................................................................+
   2024-08-08 07:56:54: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-08-08 07:56:54: %LAUT-INFO: +..............................................................................+
   Enter pattern to INCLUDE (Press enter for multiple patterns):
   (INCLUDE)> >=1000
   (INCLUDE)> <2000
   (INCLUDE)>
   (lӓut-host1) list 1
   api:
     function: get_interface_mtu_size
     arguments:
       device: host1
       interface: Loopback0
     include:
       - '>=1000'
       - <2000
   (lӓut-host1)

**list**

If an api returns a list, we can check if a particular element is present in the list via an *include* entry.
See an example below where we check if certain interfaces are present under the vrf 'red':

.. code-block:: console

   (lӓut-host1) api _get_vrf_interface -i
   Gets the subinterfaces for vrf
   
   Args:
       device ('obj'): device to run on
       vrf ('str'): vrf to search under
   
   Returns:
       interfaces('list'): List of interfaces under specified vrf
       None
   
   Raises:
       None
   
   device: [<Device host1 at 0x7fbf73857400>]
   vrf: red
   2024-08-08 08:12:21: %LAUT-INFO: +..............................................................................+
   2024-08-08 08:12:21: %LAUT-INFO: :                   Api 'get_vrf_interface' with parameters:                   :
   2024-08-08 08:12:21: %LAUT-INFO: :                                device: host1                                 :
   2024-08-08 08:12:21: %LAUT-INFO: :                                  vrf: 'red'                                  :
   2024-08-08 08:12:21: %LAUT-INFO: +..............................................................................+
   
   2024-08-08 08:12:21,975: %UNICON-INFO: +++ host1 with via 'a': executing command 'show vrf red' +++
   show vrf red
     Name                             Default RD            Protocols   Interfaces
     red                              <not set>                         Lo99
                                                                        Lo100
   host1#
   2024-08-08 08:12:22: %LAUT-INFO: +..............................................................................+
   2024-08-08 08:12:22: %LAUT-INFO: :                                 Api output:                                  :
   2024-08-08 08:12:22: %LAUT-INFO: +..............................................................................+
                                                             ['Loopback100', 'Loopback99']
   2024-08-08 08:12:22: %LAUT-INFO: +..............................................................................+
   2024-08-08 08:12:22: %LAUT-INFO: +..............................................................................+
   2024-08-08 08:12:22: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-08-08 08:12:22: %LAUT-INFO: +..............................................................................+
   Enter elements of the list you would like to INCLUDE (Press enter for multiple entries):
   INCLUDE> Loopback99
   INCLUDE> Loopback100
   INCLUDE>
   (lӓut-host1) list 1
   api:
     function: get_vrf_interface
     arguments:
       device: host1
       vrf: red
     include:
       - Loopback99
       - Loopback100
   (lӓut-host1)

**bool**

Almost all verify api's return a bool(true/false); here is an example where we check if the interface is up
by checking if the api 'verify_interface_state_up' returns true:

.. code-block:: console

   (lӓut-host1) api _verify_interface_state_up -i
   Verify interface state is up and and line protocol is up
   
   Args:
       device (`obj`): Device object
       interface (`str`): Interface name
       max_time (`int`): max time
       check_interval (`int`): check interval
   Returns:
       result(`bool`): True if is up else False
   
   device: [<Device host1 at 0x7fbf73857400>]
   interface: Ethernet0/0
   max_time(60): 10
   check_interval(10): 10
   2024-08-08 08:07:50: %LAUT-INFO: +..............................................................................+
   2024-08-08 08:07:50: %LAUT-INFO: :               Api 'verify_interface_state_up' with parameters:               :
   2024-08-08 08:07:50: %LAUT-INFO: :                                device: host1                                 :
   2024-08-08 08:07:50: %LAUT-INFO: :                           interface: 'Ethernet0/0'                           :
   2024-08-08 08:07:50: %LAUT-INFO: :                                 max_time: 10                                 :
   2024-08-08 08:07:50: %LAUT-INFO: :                              check_interval: 10                              :
   2024-08-08 08:07:50: %LAUT-INFO: +..............................................................................+
   
   2024-08-08 08:07:51,056: %UNICON-INFO: +++ host1 with via 'a': executing command 'show interfaces Ethernet0/0' +++
   show interfaces Ethernet0/0
   Ethernet0/0 is up, line protocol is up
     Hardware is AmdP2, address is aabb.cc00.0100 (bia aabb.cc00.0100)
     Internet address is 10.10.10.1/24
     MTU 1500 bytes, BW 10000 Kbit/sec, DLY 1000 usec,
        reliability 255/255, txload 1/255, rxload 1/255
     Encapsulation ARPA, loopback not set
     Keepalive set (10 sec)
     ARP type: ARPA, ARP Timeout 04:00:00
     Last input 1w5d, output 00:00:00, output hang never
     Last clearing of "show interface" counters never
     Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
     Queueing strategy: fifo
     Output queue: 0/40 (size/max)
     5 minute input rate 0 bits/sec, 0 packets/sec
     5 minute output rate 0 bits/sec, 0 packets/sec
        22118 packets input, 3638622 bytes, 0 no buffer
        Received 22102 broadcasts (0 IP multicasts)
        0 runts, 0 giants, 0 throttles
        0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
        0 input packets with dribble condition detected
        39848 packets output, 4708267 bytes, 0 underruns
        Output 22125 broadcasts (0 IP multicasts)
        0 output errors, 0 collisions, 3 interface resets
        0 unknown protocol drops
        0 babbles, 0 late collision, 0 deferred
        0 lost carrier, 0 no carrier
        0 output buffer failures, 0 output buffers swapped out
   host1#
   2024-08-08 08:07:51: %LAUT-INFO: +..............................................................................+
   2024-08-08 08:07:51: %LAUT-INFO: :                                 Api output:                                  :
   2024-08-08 08:07:51: %LAUT-INFO: +..............................................................................+
                                                                          True
   2024-08-08 08:07:51: %LAUT-INFO: +..............................................................................+
   2024-08-08 08:07:51: %LAUT-INFO: +..............................................................................+
   2024-08-08 08:07:51: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-08-08 08:07:51: %LAUT-INFO: +..............................................................................+
   Enter value to check (True/False): True
   (lӓut-host1) list 1
   api:
     function: verify_interface_state_up
     arguments:
       device: host1
       interface: Ethernet0/0
       max_time: 10
       check_interval: 10
     include:
       - true
   (lӓut-host1)

Exclude entries
---------------

To add an *exclude* entry, invoke ``api`` command with the argument '-e' which would prompt the user
for adding the *exclude* entries. *Exclude* entries also vary with the type of output as shown
in the following table:

.. list-table:: Exclude entry type w.r.t API output type
   :widths: 50 50
   :header-rows: 1

   * - API output type
     - Exclude entry type
   * - str
     - Any string
   * - dict
     - Dq query
   * - int
     - NOT PERMITTED
   * - list
     - Any value not in list
   * - bool
     - True/False
   * - None
     - Any value
