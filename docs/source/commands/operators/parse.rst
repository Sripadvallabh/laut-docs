
.. _parse:

parse
=====

Parses a command on the device loaded in LAUT shell and autogenerates pyATS
blitz *'parse'* action snippet.

.. note::

   If you are new to Genie parsers, please refer to Genie's `parse documentation <https://wwwin-enged.cisco.com/elearning/coursesp/pyats/user/parsers.html>`_.

A simple example of ``parse`` command for 'show ip route' on device 'l2switch':

.. code-block:: console

   (lӓut-leaf2-lag2) device l2switch
   (lӓut-l2switch) parse show ip route
   2024-07-09 19:39:53: %LAUT-INFO: +..............................................................................+
   2024-07-09 19:39:53: %LAUT-INFO: :                     Parse 'show ip route' on 'l2switch'                      :
   2024-07-09 19:39:53: %LAUT-INFO: +..............................................................................+

   2024-07-09 19:39:55,792: %UNICON-INFO: +++ l2switch with via 'cli': executing command 'show ip route' +++
   show ip route
   Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
          D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
          N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
          E1 - OSPF external type 1, E2 - OSPF external type 2, m - OMP
          n - NAT, Ni - NAT inside, No - NAT outside, Nd - NAT DIA
          i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
          ia - IS-IS inter area, * - candidate default, U - per-user static route
          H - NHRP, G - NHRP registered, g - NHRP registration summary
          o - ODR, P - periodic downloaded static route, l - LISP
          a - application route
          + - replicated route, % - next hop override, p - overrides from PfR
          & - replicated local route overrides by connected

   Gateway of last resort is not set

         100.0.0.0/32 is subnetted, 1 subnets
   C        100.100.100.100 is directly connected, Loopback30
   l2switch#
   2024-07-09 19:39:55: %LAUT-INFO: +..............................................................................+
   2024-07-09 19:39:55: %LAUT-INFO: :                                Parse output:                                 :
   2024-07-09 19:39:55: %LAUT-INFO: +..............................................................................+
                                     {
                                       'vrf': {
                                         'default': {
                                           'address_family': {
                                             'ipv4': {
                                               'routes': {
                                                 '100.100.100.100/32': {
                                                   'route': '100.100.100.100/32'
                                                   'active': True
                                                   'source_protocol_codes': 'C'
                                                   'source_protocol': 'connected'
                                                   'next_hop': {
                                                     'outgoing_interface': {
                                                       'Loopback30': {
                                                         'outgoing_interface': 'Loopback30'
                                                       }
                                                     }
                                                   }
                                                 }
                                               }
                                             }
                                           }
                                         }
                                       }
                                     }
   2024-07-09 19:39:55: %LAUT-INFO: +..............................................................................+
   Add INCLUDE section (y/n): n
   Add EXCLUDE section (y/n): n
   Add SAVE section (y/n): n
   (lӓut-l2switch)

The output from the command execution as well as the parse output dictionary will be
printed on the terminal screen along with a prompt requesting the user to input INCLUDE,
EXCLUDE & SAVE sections.

**INCLUDE**

This section is used to verify if something 'is' present in the parse output dictionary.
Any input verification to this section should be a Dq query. For information on Dq, see `here <https://pubhub.devnetcloud.com/media/genie-docs/docs/userguide/utils/index.html#dq>`_.

LAUT has its own syntax for these verification inputs which convert to a Dq query in the backend.
The syntax mapping is as follows:

.. _laut_query_to_dq_query:

.. list-table:: LAUT query syntax to Dq query mapping
   :widths: 50 50
   :header-rows: 1

   * - LAUT query syntax
     - Dq query
   * - a
     - contains('a')
   * - !a
     - not_contains('a')
   * - a=b
     - contains_key_value('a', 'b')
   * - !a=b
     - not_contains_key_value('a', 'b')
   * - a>1
     - value_operator('a', '>', 1)
   * - +a>1
     - sum_value_operator('a', '>', 1)
   * - g(a)
     - get_values('a')
   * - c()
     - count()
   * - r([a][b])
     - raw('[a][b]')

A simple example for Dq query to validate whether the route '100.100.100.100'
is from interface 'Loopback30' & is directly connected:

Parse dictionary output:

.. code-block:: python

    {'vrf': {'default': {'address_family': {'ipv4': {'routes': {'100.100.100.100/32': {'active': True,
                                                                                       'next_hop': {'outgoing_interface': {'Loopback30': {'outgoing_interface': 'Loopback30'}}},
                                                                                       'route': '100.100.100.100/32',
                                                                                       'source_protocol': 'connected',
                                                                                       'source_protocol_codes': 'C'}}}}}}}

Dq Queries:

.. code-block:: python

   contains('100.100.100.100/32').contains_key_value('outgoing_interface', 'Loopback30').
   contains('100.100.100.100/32').contains_key_value('source_protocol_codes', 'C')

LAUT style queries:

.. code-block:: console

   100.100.100.100/32,outgoing_interface=Loopback30
   100.100.100.100/32,source_protocol_codes=C

The ``parse`` command & autogenerated blitz snippet for the above example:

.. code-block:: console

   (lӓut-l2switch) parse show ip route
   2024-07-10 08:51:06: %LAUT-INFO: +..............................................................................+
   2024-07-10 08:51:06: %LAUT-INFO: :                     Parse 'show ip route' on 'l2switch'                      :
   2024-07-10 08:51:06: %LAUT-INFO: +..............................................................................+

   2024-07-10 08:51:07,221: %UNICON-INFO: +++ l2switch with via 'cli': executing command 'show ip route' +++
   show ip route
   Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
          D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
          N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
          E1 - OSPF external type 1, E2 - OSPF external type 2, m - OMP
          n - NAT, Ni - NAT inside, No - NAT outside, Nd - NAT DIA
          i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
          ia - IS-IS inter area, * - candidate default, U - per-user static route
          H - NHRP, G - NHRP registered, g - NHRP registration summary
          o - ODR, P - periodic downloaded static route, l - LISP
          a - application route
          + - replicated route, % - next hop override, p - overrides from PfR
          & - replicated local route overrides by connected

   Gateway of last resort is not set

         100.0.0.0/32 is subnetted, 1 subnets
   C        100.100.100.100 is directly connected, Loopback30
   l2switch#
   2024-07-10 08:51:07: %LAUT-INFO: +..............................................................................+
   2024-07-10 08:51:07: %LAUT-INFO: :                                Parse output:                                 :
   2024-07-10 08:51:07: %LAUT-INFO: +..............................................................................+
                                     {
                                       'vrf': {
                                         'default': {
                                           'address_family': {
                                             'ipv4': {
                                               'routes': {
                                                 '100.100.100.100/32': {
                                                   'route': '100.100.100.100/32'
                                                   'active': True
                                                   'source_protocol_codes': 'C'
                                                   'source_protocol': 'connected'
                                                   'next_hop': {
                                                     'outgoing_interface': {
                                                       'Loopback30': {
                                                         'outgoing_interface': 'Loopback30'
                                                       }
                                                     }
                                                   }
                                                 }
                                               }
                                             }
                                           }
                                         }
                                       }
                                     }
   2024-07-10 08:51:07: %LAUT-INFO: +..............................................................................+
   Add INCLUDE section (y/n): y
   𝟏 'vrf':
     𝟐 'default':
       𝟑 'address_family':
         𝟒 'ipv4':
           𝟓 'routes':
             𝟔 '100.100.100.100/32':
               𝟕 'route': '100.100.100.100/32'
               𝟖 'active': True
               𝟗 'source_protocol_codes': 'C'
               𝟏𝟎 'source_protocol': 'connected'
               𝟏𝟏 'next_hop':
                 𝟏𝟐 'outgoing_interface':
                   𝟏𝟑 'Loopback30':
                     𝟏𝟒 'outgoing_interface': 'Loopback30'
                   }
                 }
               }
             }
           }
         }
       }
     }
   }
   Enter q-style query (or) line numbers (Press enter for multiple entries):
   INCLUDE> 100.100.100.100/32,outgoing_interface=Loopback30
   {'vrf': {'default': {'address_family': {'ipv4': {'routes': {'100.100.100.100/32': {'next_hop': {'outgoing_interface': {'Loopback30': {'outgoing_interface': 'Loopback30'}}}}}}}}}}
   Do you wish to add this Dq query (y/n): y
   INCLUDE> 100.100.100.100/32,source_protocol_codes=C
   {'vrf': {'default': {'address_family': {'ipv4': {'routes': {'100.100.100.100/32': {'source_protocol_codes': 'C'}}}}}}}
   Do you wish to add this Dq query (y/n): y
   INCLUDE>
   Add EXCLUDE section (y/n): n
   Add SAVE section (y/n): n
   (lӓut-l2switch) list 1
               - parse:
                   device: l2switch
                   command: show ip route
                   include:
                       - "contains('100.100.100.100/32').contains_key_value('outgoing_interface', 'Loopback30')"
                       - "contains('100.100.100.100/32').contains_key_value('source_protocol_codes', 'C')"
   (lӓut-l2switch)

Multiple INCLUDE entries were given by pressing <enter> & when given a LAUT styled query to INCLUDE section, it will
always reconstruct the query & prints the result for the user to validate the query before accepting it. After
the two Dq queries were added, a third <enter> was given to exit the *'INCLUDE>'* mode. Note
the autogenerated *'parse'* blitz action contains the two Dq queries as its *'include'* entry.

Since the concept of Dq query is slightly hard to grasp, LAUT also has the *'line no method'* to
generate Dq queries for the user. This is precisely why after entering an INCLUDE section, the parse output
dictionary was again printed & this time each key and key-value pair was printed with a line number preceding it.
Hence all the user needs to give to INCLUDE section is the corresponding line number that matches the key value
pair needed to be verified.

From the same example above, here is the INCLUDE parse dictionary output:

.. code-block:: console

   Add INCLUDE section (y/n): y
   𝟏 'vrf':
     𝟐 'default':
       𝟑 'address_family':
         𝟒 'ipv4':
           𝟓 'routes':
             𝟔 '100.100.100.100/32':
               𝟕 'route': '100.100.100.100/32'
               𝟖 'active': True
               𝟗 'source_protocol_codes': 'C'
               𝟏𝟎 'source_protocol': 'connected'
               𝟏𝟏 'next_hop':
                 𝟏𝟐 'outgoing_interface':
                   𝟏𝟑 'Loopback30':
                     𝟏𝟒 'outgoing_interface': 'Loopback30'

The line numbers for the Dq queries would be as follows:

.. list-table:: LAUT query to Dq query mapping
   :header-rows: 1

   * - Dq query
     - Line number
   * - contains('100.100.100.100/32').contains_key_value('outgoing_interface', 'Loopback30')
     - 14
   * - contains('100.100.100.100/32').contains_key_value('source_protocol_codes', 'C')
     - 9

Once the line numbers are given to INCLUDE section in the format '#<line_no1>,<line_no2>',
LAUT would generate the Dq queries as seen below from ``list 1``:

.. code-block:: console

   (lӓut-l2switch) parse show ip route
   2024-07-10 09:10:48: %LAUT-INFO: +..............................................................................+
   2024-07-10 09:10:48: %LAUT-INFO: :                     Parse 'show ip route' on 'l2switch'                      :
   2024-07-10 09:10:48: %LAUT-INFO: +..............................................................................+

   2024-07-10 09:10:49,356: %UNICON-INFO: +++ l2switch with via 'cli': executing command 'show ip route' +++
   show ip route
   Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
          D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
          N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
          E1 - OSPF external type 1, E2 - OSPF external type 2, m - OMP
          n - NAT, Ni - NAT inside, No - NAT outside, Nd - NAT DIA
          i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
          ia - IS-IS inter area, * - candidate default, U - per-user static route
          H - NHRP, G - NHRP registered, g - NHRP registration summary
          o - ODR, P - periodic downloaded static route, l - LISP
          a - application route
          + - replicated route, % - next hop override, p - overrides from PfR
          & - replicated local route overrides by connected

   Gateway of last resort is not set

         100.0.0.0/32 is subnetted, 1 subnets
   C        100.100.100.100 is directly connected, Loopback30
   l2switch#
   2024-07-10 09:10:49: %LAUT-INFO: +..............................................................................+
   2024-07-10 09:10:49: %LAUT-INFO: :                                Parse output:                                 :
   2024-07-10 09:10:49: %LAUT-INFO: +..............................................................................+
                                     {
                                       'vrf': {
                                         'default': {
                                           'address_family': {
                                             'ipv4': {
                                               'routes': {
                                                 '100.100.100.100/32': {
                                                   'route': '100.100.100.100/32'
                                                   'active': True
                                                   'source_protocol_codes': 'C'
                                                   'source_protocol': 'connected'
                                                   'next_hop': {
                                                     'outgoing_interface': {
                                                       'Loopback30': {
                                                         'outgoing_interface': 'Loopback30'
                                                       }
                                                     }
                                                   }
                                                 }
                                               }
                                             }
                                           }
                                         }
                                       }
                                     }
   2024-07-10 09:10:49: %LAUT-INFO: +..............................................................................+
   Add INCLUDE section (y/n): y
   𝟏 'vrf':
     𝟐 'default':
       𝟑 'address_family':
         𝟒 'ipv4':
           𝟓 'routes':
             𝟔 '100.100.100.100/32':
               𝟕 'route': '100.100.100.100/32'
               𝟖 'active': True
               𝟗 'source_protocol_codes': 'C'
               𝟏𝟎 'source_protocol': 'connected'
               𝟏𝟏 'next_hop':
                 𝟏𝟐 'outgoing_interface':
                   𝟏𝟑 'Loopback30':
                     𝟏𝟒 'outgoing_interface': 'Loopback30'
                   }
                 }
               }
             }
           }
         }
       }
     }
   }
   }
   Enter q-style query (or) line numbers (Press enter for multiple entries): #9,14
   Add EXCLUDE section (y/n): n
   Add SAVE section (y/n): n
   (lӓut-l2switch) list 1
               - parse:
                   device: l2switch
                   command: show ip route
                   include:
                       - "contains_key_value('source_protocol_codes', 'C').contains('100.100.100.100/32').contains('routes').contains('ipv4').contains('address_family').contains('default').contains('vrf')"
                       - "contains_key_value('outgoing_interface', 'Loopback30').contains('Loopback30').contains('outgoing_interface').contains('next_hop').contains('100.100.100.100/32').contains('routes').contains('ipv4').contains('address_family').contains('default').contains('vrf')"
   (lӓut-l2switch)

The corresponding Dq queries from the line numbers are added as *'include'* entries. The only difference between
this & the user given Dq query is that since we do not know which are the important keys to access the keys downstream,
all keys are added to the Dq query but this is equivalent to the user query seen above.

.. note::

   Line numbers to Dq query mapping was not the intent behind us LAUT developers. Line numbers to partial
   dictionary was the intent, but since partial dictionary match was not supported in blitz INCLUDE
   sections, it was shifted to generate a Dq query for the time being. After discussion with blitz
   team for the same requirement, there can be a shift back to partial dictionary match since its more readable.

**EXCLUDE**

Behaves in the same way as an INCLUDE section, except that it checks if a particular
Dq query DOES NOT match the ``parse`` dictionary output.

Continuing with the previous example, if we *shut* the interface 'Loopback30' it is expected that
the route to '100.100.100.100' is expected to not show up in 'show ip route' command. For this
particular case, we should input the same Dq queries we gave earlier to INCLUDE section rather now into
the EXCLUDE section so as to verify that after the interface has been *shut* we shouldn't expect the
route to be present in the routing table:

.. code-block:: console

   (lӓut-l2switch) configure
   (l2switch:config)> interface Loopback30
   (l2switch:config-if)> shutdown
   (l2switch:config-if)> end
   (lӓut-l2switch) parse show ip route
   2024-07-10 09:26:02: %LAUT-INFO: +..............................................................................+
   2024-07-10 09:26:02: %LAUT-INFO: :                     Parse 'show ip route' on 'l2switch'                      :
   2024-07-10 09:26:02: %LAUT-INFO: +..............................................................................+

   2024-07-10 09:26:02,838: %UNICON-INFO: +++ l2switch with via 'cli': executing command 'show ip route' +++
   show ip route
   Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
          D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
          N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
          E1 - OSPF external type 1, E2 - OSPF external type 2, m - OMP
          n - NAT, Ni - NAT inside, No - NAT outside, Nd - NAT DIA
          i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
          ia - IS-IS inter area, * - candidate default, U - per-user static route
          H - NHRP, G - NHRP registered, g - NHRP registration summary
          o - ODR, P - periodic downloaded static route, l - LISP
          a - application route
          + - replicated route, % - next hop override, p - overrides from PfR
          & - replicated local route overrides by connected

   Gateway of last resort is not set

         5.0.0.0/32 is subnetted, 1 subnets
   C        5.5.5.5 is directly connected, Loopback0
   l2switch#
   2024-07-10 09:26:02: %LAUT-INFO: +..............................................................................+
   2024-07-10 09:26:02: %LAUT-INFO: :                                Parse output:                                 :
   2024-07-10 09:26:02: %LAUT-INFO: +..............................................................................+
                                     {
                                       'vrf': {
                                         'default': {
                                           'address_family': {
                                             'ipv4': {
                                               'routes': {
                                                 '5.5.5.5/32': {
                                                   'route': '5.5.5.5/32'
                                                   'active': True
                                                   'source_protocol_codes': 'C'
                                                   'source_protocol': 'connected'
                                                   'next_hop': {
                                                     'outgoing_interface': {
                                                       'Loopback0': {
                                                         'outgoing_interface': 'Loopback0'
                                                       }
                                                     }
                                                   }
                                                 }
                                               }
                                             }
                                           }
                                         }
                                       }
                                     }
   2024-07-10 09:26:02: %LAUT-INFO: +..............................................................................+
   Add INCLUDE section (y/n): n
   Add EXCLUDE section (y/n): y
   Enter q-style query (Press enter for multiple entries):
   EXCLUDE> 100.100.100.100/32,outgoing_interface=Loopback30
   {}
   Do you wish to add this Dq query (y/n): y
   EXCLUDE> 100.100.100.100/32,source_protocol_codes=C
   {}
   Do you wish to add this Dq query (y/n): y
   EXCLUDE>
   Add SAVE section (y/n): n
   (lӓut-l2switch) list 1
               - parse:
                   device: l2switch
                   command: show ip route
                   exclude:
                       - "contains('.*100.100.100.100/32.*', regex=True).contains_key_value('outgoing_interface', '.*Loopback30.*', value_regex=True)"
                       - "contains('.*100.100.100.100/32.*', regex=True).contains_key_value('source_protocol_codes', 'C')"
   (lӓut-l2switch)

Multiple EXCLUDE entries were given by pressing <enter> & when given the LAUT styled query to EXCLUDE section,
it will always reconstruct the query & prints the result much in the same way as an INCLUDE section, and once
we verify that the result printed is '{}' (which essentially means that no matches were found for the given query)
the user can accept it; this prevents invalid queries from being added without validation. Note the autogenerated
*'parse'* blitz action contains the two Dq queries as its *'exclude'* entry.

**SAVE**

.. note::

   For first time readers, skip this section altogether.
   Read only after going through 'Variables & Parameters' in LAUT features section first.

YET TO BE ADDED
