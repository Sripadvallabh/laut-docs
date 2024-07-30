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
the user needs to type each of them one by one. Default parameter values are shown in parenthesis after their
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
passed as seen above. From the 'LAUT-INFO' messages printed for the above example, we could infer that indeed the
'option' and 'output' parameters were passed 'None'.

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
     - Exact string output
   * - dict
     - Dq query, Line numbers
   * - int
     - Mathematical expression {==,>=,<=,<,>,!=}
   * - list
     - List values
   * - bool
     - True/False
   * - None
     - No *include* entry permitted

*Include* entries for type 'dict' are similar to ``parse``; see :doc:`parse` for more information.

Exclude entries
---------------

To add an *exclude* entry, invoke ``api`` command with the argument '-e' which would prompt the user
for adding the *exclude* entries. *Exclude* entries also vary with the type of output as shown
in the following table:

.. list-table:: Exclude entry type w.r.t API output type
   :widths: 50 50
   :header-rows: 1

   * - API output type
     - Include entry type
   * - str
     - Any string
   * - dict
     - Dq query
   * - int
     - No *include* entry permitted
   * - list
     - Any value
   * - bool
     - True/False
   * - None
     - Any value
