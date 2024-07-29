parse
=====

Parses a command on the device loaded in LAUT shell using Genieparser and can autogenerate pyATS
blitz *'parse'* action snippet.

.. note::

   If you are new to Genie parsers, please refer to Genie's `parser documentation <https://wwwin-enged.cisco.com/elearning/coursesp/pyats/user/parsers.html>`_.

Why parse
---------
``parse`` is faster to verify command outputs than ``execute``. A regex pattern to check outputs is very
tedious and difficult to maintain and understand, at the same time being less precise. A parsed dictionary
can be verified much more accurately and in a faster way with the hacks provided by LAUT as seen below.

Basic usage
------------

A simple example of ``parse`` command for 'show ip route' on device 'host2':

.. code-block:: console

   (lӓut-host2) parse show ip route
   2024-07-28 10:09:58: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:09:58: %LAUT-INFO: :                       Parse 'show ip route' on 'host2'                       :
   2024-07-28 10:09:58: %LAUT-INFO: +..............................................................................+
   
   2024-07-28 10:09:58,684: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip route' +++
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
   
         1.0.0.0/32 is subnetted, 1 subnets
   C        1.1.1.1 is directly connected, Loopback0
   host2#
   2024-07-28 10:09:58: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:09:58: %LAUT-INFO: :                                Parse output:                                 :
   2024-07-28 10:09:58: %LAUT-INFO: +..............................................................................+
                                     {
                                       'vrf': {
                                         'default': {
                                           'address_family': {
                                             'ipv4': {
                                               'routes': {
                                                 '1.1.1.1/32': {
                                                   'route': '1.1.1.1/32'
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
   2024-07-28 10:09:58: %LAUT-INFO: +..............................................................................+
   (lӓut-host2)

The output from the command execution as well as the parse output dictionary will be
printed on the terminal screen. LAUT does not autogenerate blitz *'parse'* action snippet when
calling ``parse``, rather it only does so when adding an *include* or *exclude* entry as seen below.

Include entries
---------------

A ``parse`` command's *include* entry can be used to verify if something 'is' present in the parse output dictionary.
Any input verification to this section should be a Dq query. For information on Dq, see `here <https://pubhub.devnetcloud.com/media/genie-docs/docs/userguide/utils/index.html#dq>`_.

To add a parse *include* entry, invoke ``parse`` with the optional argument '-i' which would prompt user for
the *include* entries after the parse dictionary of the command is printed on the screen.

A simple example for a Dq query to validate whether the route '1.1.1.1' is from interface 'Loopback0' &
is directly connected:

Parse dictionary output from earlier:

.. code-block:: python

   {
     'vrf': {
       'default': {
         'address_family': {
           'ipv4': {
             'routes': {
               '1.1.1.1/32': {
         	'route': '1.1.1.1/32'
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

The corresponding Dq Queries to verify both the 'Loopback0' interface & the 'C' route would look like this:

.. code-block:: python

   contains('1.1.1.1/32').contains_key_value('outgoing_interface', 'Loopback0')
   contains('1.1.1.1/32').contains_key_value('source_protocol_codes', 'C')

The ``parse`` command & the autogenerated blitz snippet along with the *include* entries for the above example:

.. code-block:: console

   (lӓut-host2) parse show ip route -i
   2024-07-28 10:22:34: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:22:34: %LAUT-INFO: :                       Parse 'show ip route' on 'host2'                       :
   2024-07-28 10:22:34: %LAUT-INFO: +..............................................................................+
   
   2024-07-28 10:22:35,119: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip route' +++
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
   
         1.0.0.0/32 is subnetted, 1 subnets
   C        1.1.1.1 is directly connected, Loopback0
   host2#
   2024-07-28 10:22:35: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:22:35: %LAUT-INFO: :                                Parse output:                                 :
   2024-07-28 10:22:35: %LAUT-INFO: +..............................................................................+
                                     {
                                       'vrf': {
                                         'default': {
                                           'address_family': {
                                             'ipv4': {
                                               'routes': {
                                                 '1.1.1.1/32': {
                                                   'route': '1.1.1.1/32'
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
   2024-07-28 10:22:35: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:22:35: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:22:35: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-07-28 10:22:35: %LAUT-INFO: +..............................................................................+
   𝟏 'vrf':
     𝟐 'default':
       𝟑 'address_family':
         𝟒 'ipv4':
           𝟓 'routes':
             𝟔 '1.1.1.1/32':
               𝟕 'route': '1.1.1.1/32'
               𝟖 'active': True
               𝟗 'source_protocol_codes': 'C'
               𝟏𝟎 'source_protocol': 'connected'
               𝟏𝟏 'next_hop':
                 𝟏𝟐 'outgoing_interface':
                   𝟏𝟑 'Loopback0':
                     𝟏𝟒 'outgoing_interface': 'Loopback0'
   Enter Dq query (or) line numbers (Press enter for multiple entries):
   INCLUDE> contains('1.1.1.1/32').contains_key_value('outgoing_interface', 'Loopback0')
   {'vrf': {'default': {'address_family': {'ipv4': {'routes': {'1.1.1.1/32': {'next_hop': {'outgoing_interface': {'Loopback0': {'outgoing_interface': 'Loopback0'}}}}}}}}}}
   Do you wish to add this Dq query (y/n): y
   INCLUDE> contains('1.1.1.1/32').contains_key_value('source_protocol_codes', 'C')
   {'vrf': {'default': {'address_family': {'ipv4': {'routes': {'1.1.1.1/32': {'source_protocol_codes': 'C'}}}}}}}
   Do you wish to add this Dq query (y/n): y
   INCLUDE>
   (lӓut-host2) list 1
   parse:
     device: host2
     command: show ip route
     include:
       - contains('1.1.1.1/32').contains_key_value('outgoing_interface', 'Loopback0')
       - contains('1.1.1.1/32').contains_key_value('source_protocol_codes', 'C')
   (lӓut-host2)

Multiple *include* entries were given by pressing <enter>.After the two Dq queries were added, a third <enter> was given to exit the *'INCLUDE>'* mode. Note the autogenerated *'parse'* blitz action contains the two Dq queries as a list under blitz *'include'*.

When giving a Dq query as an *include* entry, LAUT always reconstructs a dictionary from the query and displays
it for the user to validate the query before accepting it. The reconstructed dictionary
represents the actual data from the parsed output dictionary that we would like to verify.
If the reconstructed dict is equal to an empty dict({}), it would mean that the query does not 
match the given parse dictionary.


**Dq query shorthand**

LAUT has a shorthand syntax for typing these Dq queries which in turn re-convert back to a Dq query.
The syntax mapping is as follows(with multiple individual elements combined with ',' similar to '.' in Dq query)

.. _dq_query_shorthand:

.. list-table:: Dq shorthand syntax to Dq query mapping
   :widths: 50 50
   :header-rows: 1

   * - Dq shorthand syntax
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

Using the mapping table given above, the shorthand for the previously mentioned two Dq queries are shown below:

.. code-block:: console

   1.1.1.1/32,outgoing_interface=Loopback0
   1.1.1.1/32,source_protocol_codes=C

The corresponding ``parse`` command output with the *include* entries in the form of Dq shorthand syntax are shown below:

.. code-block:: console

   (lӓut-host2) parse show ip route -i
   2024-07-28 10:27:50: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:27:50: %LAUT-INFO: :                       Parse 'show ip route' on 'host2'                       :
   2024-07-28 10:27:50: %LAUT-INFO: +..............................................................................+
   
   2024-07-28 10:27:50,406: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip route' +++
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
   
         1.0.0.0/32 is subnetted, 1 subnets
   C        1.1.1.1 is directly connected, Loopback0
   host2#
   2024-07-28 10:27:50: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:27:50: %LAUT-INFO: :                                Parse output:                                 :
   2024-07-28 10:27:50: %LAUT-INFO: +..............................................................................+
                                     {
                                       'vrf': {
                                         'default': {
                                           'address_family': {
                                             'ipv4': {
                                               'routes': {
                                                 '1.1.1.1/32': {
                                                   'route': '1.1.1.1/32'
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
   2024-07-28 10:27:50: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:27:50: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:27:50: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-07-28 10:27:50: %LAUT-INFO: +..............................................................................+
   𝟏 'vrf':
     𝟐 'default':
       𝟑 'address_family':
         𝟒 'ipv4':
           𝟓 'routes':
             𝟔 '1.1.1.1/32':
               𝟕 'route': '1.1.1.1/32'
               𝟖 'active': True
               𝟗 'source_protocol_codes': 'C'
               𝟏𝟎 'source_protocol': 'connected'
               𝟏𝟏 'next_hop':
                 𝟏𝟐 'outgoing_interface':
                   𝟏𝟑 'Loopback0':
                     𝟏𝟒 'outgoing_interface': 'Loopback0'
   Enter Dq query (or) line numbers (Press enter for multiple entries):
   INCLUDE> 1.1.1.1/32,outgoing_interface=Loopback0
   {'vrf': {'default': {'address_family': {'ipv4': {'routes': {'1.1.1.1/32': {'next_hop': {'outgoing_interface': {'Loopback0': {'outgoing_interface': 'Loopback0'}}}}}}}}}}
   Do you wish to add this Dq query (y/n): y
   INCLUDE> 1.1.1.1/32,source_protocol_codes=C
   {'vrf': {'default': {'address_family': {'ipv4': {'routes': {'1.1.1.1/32': {'source_protocol_codes': 'C'}}}}}}}
   Do you wish to add this Dq query (y/n): y
   INCLUDE>
   (lӓut-host2) list 1
   parse:
     device: host2
     command: show ip route
     include:
       - contains('1.1.1.1/32').contains_key_value('outgoing_interface', 'Loopback0')
       - contains('1.1.1.1/32').contains_key_value('source_protocol_codes', 'C')
   (lӓut-host2)

Notice from ``list 1`` output that the Dq queries in blitz *'include'* are exactly the same
as the Dq queries we intended to generate from the Dq shorthand syntax. Hence, it is always recommended
to use the Dq shorthand instead of plain Dq.

**Line numbers**

Since the concept of Dq query is slightly hard to grasp, LAUT also has the *'line no method'* to
generate Dq queries for the user. This is precisely why when ``parse <CMD> -i`` is invoked, the parse output
dictionary was again printed & this time each key and key-value pair was printed with a line number preceding it.
Hence all the user needs to give is the corresponding line number that matches the key value
pair needed to be verified.

From the same example above, here is the parse dictionary output with the line numbers printed:

.. code-block:: console

   2024-07-28 10:22:35: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:22:35: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-07-28 10:22:35: %LAUT-INFO: +..............................................................................+
   𝟏 'vrf':
     𝟐 'default':
       𝟑 'address_family':
         𝟒 'ipv4':
           𝟓 'routes':
             𝟔 '1.1.1.1/32':
               𝟕 'route': '1.1.1.1/32'
               𝟖 'active': True
               𝟗 'source_protocol_codes': 'C'
               𝟏𝟎 'source_protocol': 'connected'
               𝟏𝟏 'next_hop':
                 𝟏𝟐 'outgoing_interface':
                   𝟏𝟑 'Loopback0':
                     𝟏𝟒 'outgoing_interface': 'Loopback0'

The line numbers for the Dq queries would be as follows:

.. list-table:: LAUT query to Dq query mapping
   :header-rows: 1

   * - Dq query
     - Line number
   * - contains('1.1.1.1/32').contains_key_value('outgoing_interface', 'Loopback0')
     - 14
   * - contains('1.1.1.1/32').contains_key_value('source_protocol_codes', 'C')
     - 9

Once the line numbers are given as an *include* entry in the format '#<line_no>'(with
multiple line numbers in the format '#<line_no1>,<line_no2>'),
LAUT would generate the Dq queries as seen below from ``list 1``:

.. code-block:: console

   (lӓut-host2) parse show ip route -i
   2024-07-28 10:31:07: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:31:07: %LAUT-INFO: :                       Parse 'show ip route' on 'host2'                       :
   2024-07-28 10:31:07: %LAUT-INFO: +..............................................................................+
   
   2024-07-28 10:31:07,962: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip route' +++
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
   
         1.0.0.0/32 is subnetted, 1 subnets
   C        1.1.1.1 is directly connected, Loopback0
   host2#
   2024-07-28 10:31:08: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:31:08: %LAUT-INFO: :                                Parse output:                                 :
   2024-07-28 10:31:08: %LAUT-INFO: +..............................................................................+
                                     {
                                       'vrf': {
                                         'default': {
                                           'address_family': {
                                             'ipv4': {
                                               'routes': {
                                                 '1.1.1.1/32': {
                                                   'route': '1.1.1.1/32'
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
   2024-07-28 10:31:08: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:31:08: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:31:08: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-07-28 10:31:08: %LAUT-INFO: +..............................................................................+
   𝟏 'vrf':
     𝟐 'default':
       𝟑 'address_family':
         𝟒 'ipv4':
           𝟓 'routes':
             𝟔 '1.1.1.1/32':
               𝟕 'route': '1.1.1.1/32'
               𝟖 'active': True
               𝟗 'source_protocol_codes': 'C'
               𝟏𝟎 'source_protocol': 'connected'
               𝟏𝟏 'next_hop':
                 𝟏𝟐 'outgoing_interface':
                   𝟏𝟑 'Loopback0':
                     𝟏𝟒 'outgoing_interface': 'Loopback0'
   Enter Dq query (or) line numbers (Press enter for multiple entries): #9,14
   (lӓut-host2)
   (lӓut-host2) list 1
   parse:
     device: host2
     command: show ip route
     include:
       - contains_key_value('source_protocol_codes',
         'C').contains('1.1.1.1/32').contains('routes').contains('ipv4').contains('address_family').contains('default').contains('vrf')
       - contains_key_value('outgoing_interface',
         'Loopback0').contains('Loopback0').contains('outgoing_interface').contains('next_hop').contains('1.1.1.1/32').contains('routes').contains('ipv4').contains('address_family').contains('default').contains('vrf')
   (lӓut-host2)


The corresponding Dq queries from the line numbers are added as a list under blitz *'include'*. The only difference between
this & the user given Dq query is that since we do not know which are the important keys to access the data downstream,
all parent keys are added to the Dq query.

.. note::

   Line numbers to Dq query mapping was not the intent behind us LAUT developers. Line numbers to partial
   dictionary was the intent, but since partial dictionary match was not supported in blitz *'include'*,
   it was shifted to generate a Dq query for the time being. After discussion with blitz
   team for the same requirement, there can be a shift back to partial dictionary match since its more readable.

Exclude entries
---------------

Behaves in the same way as an *include* entry, except that it checks if a particular
Dq query **DOES NOT** match the ``parse`` dictionary output. Add *exclude* entries by invoking ``parse``
with the extra argument '-e'. *exclude* entries are always Dq queries but can also be given in the shorthand form.

Continuing with the previous example, if we *shut* the interface 'Loopback0' it is expected that
the route to '1.1.1.1' is expected to not show up in 'show ip route' command. For this
particular case, we should input the same Dq queries we gave earlier as *include* entries rather now as
an *exclude* entry so as to verify that after the interface has been *shut* we shouldn't expect the
route to be present in the routing table:

.. code-block:: console

   (lӓut-host2) configure
   (host2:config)> interface Loopback0
   (host2:config-if)> shutdown
   (host2:config-if)> end
   (lӓut-host2) parse show ip route -e
   2024-07-28 10:50:59: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:50:59: %LAUT-INFO: :                       Parse 'show ip route' on 'host2'                       :
   2024-07-28 10:50:59: %LAUT-INFO: +..............................................................................+
   
   2024-07-28 10:51:00,050: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip route' +++
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
   
         2.0.0.0/32 is subnetted, 1 subnets
   C        2.2.2.2 is directly connected, Loopback1
   host2#
   2024-07-28 10:51:00: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:51:00: %LAUT-INFO: :                                Parse output:                                 :
   2024-07-28 10:51:00: %LAUT-INFO: +..............................................................................+
                                     {
                                       'vrf': {
                                         'default': {
                                           'address_family': {
                                             'ipv4': {
                                               'routes': {
                                                 '2.2.2.2/32': {
                                                   'route': '2.2.2.2/32'
                                                   'active': True
                                                   'source_protocol_codes': 'C'
                                                   'source_protocol': 'connected'
                                                   'next_hop': {
                                                     'outgoing_interface': {
                                                       'Loopback1': {
                                                         'outgoing_interface': 'Loopback1'
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
   2024-07-28 10:51:00: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:51:00: %LAUT-INFO: +..............................................................................+
   2024-07-28 10:51:00: %LAUT-INFO: :                                   EXCLUDE                                    :
   2024-07-28 10:51:00: %LAUT-INFO: +..............................................................................+
   Enter Dq query (Press enter for multiple entries):
   EXCLUDE> contains('1.1.1.1/32').contains_key_value('outgoing_interface', 'Loopback0')
   {}
   Do you wish to add this Dq query (y/n): y
   EXCLUDE> contains('1.1.1.1/32').contains_key_value('source_protocol_codes', 'C')
   {}
   Do you wish to add this Dq query (y/n): y
   EXCLUDE>
   (lӓut-host2)
   (lӓut-host2) list 1
   parse:
     device: host2
     command: show ip route
     exclude:
       - contains('1.1.1.1/32').contains_key_value('outgoing_interface', 'Loopback0')
       - contains('1.1.1.1/32').contains_key_value('source_protocol_codes', 'C')
   (lӓut-host2)

Multiple *exclude* entries can be given by pressing <enter> & when given a Dq query,
LAUT will always reconstruct the query & prints the reconstructed dict output, and then once
we verify that the result printed is '{}' (which essentially means that no matches were found for the given query)
the user can accept it; this prevents invalid queries from being added without validation. Note the autogenerated
*'parse'* blitz action contains the two Dq queries as a list under blitz *'exclude'*.
