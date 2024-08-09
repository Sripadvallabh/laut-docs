Autocompletion
===============

Autocompletion helps users type commands easily. <TAB> is the autocomplete key.
In case there are multiple entries to autocomplete <TAB> may not autocomplete anything,
in this case press <TAB><TAB> to view the list of autocompletions.

Autocompletion command-wise:

``device``
----------

LAUT can autocomplete device names from testbed.

.. code-block:: console

   (lӓut-host1) device host
   host1    host2

``testbed`` & ``replay``
------------------------

LAUT provides filepath autocomplete.

.. code-block:: console

   (lӓut-host1) testbed ../../alternate_msdp/
   destroy_iol                                          nvram_00013
   extra/                                               nvram_00014
   i86bi_linux-tpgen_adventerprisek9-ms.PAGENT.5.0.0    pnp-info/
   iol-linux-manual-startup                             pnp-tech/
   laut.bin                                             testbed.yaml
   mvpn-inter-as-ut-topo/                               undo.undo
   NETMAP                                               vimsession.vim
   nvram_00011                                          x86_64bi_linux-adventerprise-ms
   nvram_00012                                          xwrapper
   (lӓut-host1) testbed ../../alternate_msdp/

``api``
-------

LAUT provides module-wise and name-wise autocomplete.

Module-wise:

.. code-block:: console

   (lӓut-host1) api vrf configure
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

Name-wise:

.. code-block:: console

   (lӓut-host1) api _get_vrf_
   _get_vrf_interface              _get_vrf_route_distinguisher    _get_vrf_route_targets          _get_vrf_vrfs
   (lӓut-host1) api _get_vrf_

``parse``
---------

LAUT provides command autocompletion using the available list of parsers. Please kindly
ignore the '{}' which are meant to be values rather than keywords (Note {protocol} and {route} in the below example)

.. code-block:: console

   (lӓut-host1) parse show ip route
   summary           supernets-only    vrf               {protocol}        {route}
   (lӓut-host1) parse show ip route

``execute``
-----------

LAUT does command autocompletion for ``execute`` by transmitting and receiving the '?' symbol via
unicon API's. This takes up bandwidth, if your devices are too slow please consider to avoid pressing
<TAB>.

Autocompletion for the command ``show ip``:

.. code-block:: console

   (lӓut-leaf2) show ip
   
   COMMAND              Description
   ==========================================================================
   access-lists         List IP access lists
   accounting           The active IP accounting database
   admission            Network Admission Control information
   aliases              IP alias table
   amt                  Show AMT protocol parameters
   arp                  IP ARP table
   as-path-access-list  List AS path access lists
   bgp                  BGP information
   cache                IP fast-switching route cache
   cef                  Cisco Express Forwarding
   community-list       List community-list
   ddns                 Dynamic DNS
   dhcp                 Show items in the DHCP database
   dns                  Show DNS information
   eigrp                Show IPv4 EIGRP
   explicit-paths       Show IP explicit paths
   extcommunity-list    List extended-community list
   helper-address       helper-address table
   host-list            Host list
   <TRUNCATED>

``configure``
-------------

Autocompletion for CLI are available in LAUT-cfg mode:

.. code-block:: console

   (leaf2:config)> ip pim
   
   CONFIG                 Description
   ==============================================================================
   accept-register        Registers accept filter
   accept-rp              RP accept filter
   allow-rp               Sparse-Mode RP addresses to be allowed
   autorp                 Configure AutoRP global operations
   bidir-enable           Enable Bidir-PIM
   bidir-offer-interval   DF election offer message interval
   bidir-offer-limit      number of unanswered offers before becoming DF
   bsr-candidate          Candidate bootstrap router (candidate BSR)
   cache                  PIM cache configuration
   dm-fallback            Fallback group mode is Dense
   fast-register-stop     Immediately send register-stops on registers
   log-neighbor-changes   Log PIM neighbor up/down and DR changes
   maximum                Maximum state limits
   mpls                   pim mpls commands
   old-register-checksum  Generate Register checksum on whole packet
   register-rate-limit    Rate limit for PIM data registers
   register-source        Source address for PIM Register
   rp-address             PIM RP-address (Rendezvous Point)
   rp-announce-filter     Auto-RP announce message filter
   rp-candidate           To be a PIMv2 RP candidate
   rp-proxy-join          RP always proxy joins for sources
   send-rp-announce       Auto-RP send RP announcement
   send-rp-discovery      Auto-RP send RP discovery message (as RP-mapping agent)
   sparse                 This command is specific to PIM-Sparse Mode
   spt-threshold          Source-tree switching threshold
   ssm                    Configure Source Specific Multicast
   state-refresh          PIM DM State-Refresh configuration
   v1-rp-reachability     Send PIMv1 RP-reachability packet
   vrf                    Select VPN Routing/Forwarding instance

``list`` & ``remove``
---------------------

``list -n`` and ``remove -n`` can autocomplete with the added test section names.

.. code-block:: console

   (lӓut-leaf2) remove -n
   default    new        new_2
