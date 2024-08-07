Blitz additionals
=================

Blitz retry
-----------

This feature is to have pyATS blitz, retry an *include* or *exclude* entry after sleeping for
certain interval of time. This is useful for certain scenarios where an *'execute'* or *'parse'*
action would not have an entry at the time it is invoked but might make get the entry after
it has waited for certain period.

This feature is not possible in LAUT; the user should wait till the *include* or *exclude*
entry appears before trying to add it to the autogenerated blitz snippet as this is the way
LAUT is designed(i.e. because it is 'live'). However, to provide this information to blitz, LAUT has
a setting 'blitz_retry' to add the *'max_time'* and *'check_interval'* keys to autogenerated
blitz snippets for pyATS blitz to understand the retry mechanism user wants it to follow when
run later via ``replay`` command or externally via ``pyats run job``.

'blitz_retry' setting should be set in the format 'max_time/check_interval'. For example
to retry for 10 times with an interval of 10 seconds between them, the max_time would be (10 * 10 = 100)
with the check_interval being 10:

.. code-block:: console

   (lӓut-leaf2) set blitz_retry 100/10
   blitz_retry - was: '0/0'
   now: '100/10'
   (lӓut-leaf2)

An example where the keys 'max_time' and 'check_interval' are added to an ``execute -i``:

.. code-block:: console

   (lӓut-leaf2) show ip route -i
   2024-08-06 17:19:21: %LAUT-INFO: +..............................................................................+
   2024-08-06 17:19:21: %LAUT-INFO: :                      Execute 'show ip route' on 'leaf2'                      :
   2024-08-06 17:19:21: %LAUT-INFO: +..............................................................................+
   
   2024-08-06 17:19:21,523: %UNICON-INFO: +++ leaf2 with via 'a': executing command 'show ip route' +++
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
   O        1.1.1.1 [110/3] via 20.1.1.1, 01:20:19, GigabitEthernet1/0/1
   leaf2#
   2024-08-06 17:19:21: %LAUT-INFO: +..............................................................................+
   2024-08-06 17:19:21: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-08-06 17:19:21: %LAUT-INFO: +..............................................................................+
   Enter pattern to INCLUDE (Press enter for multiple patterns): 1.1.1.1
   (lӓut-leaf2) list 1
   execute:
     device: leaf2
     command: show ip route
     include:
       - 1.1.1.1
     max_time: 100
     check_interval: 10
   (lӓut-leaf2)

Expected failure
-----------------

This feature is mainly to do negative testing where a failed action would convert to a pass upon
having the 'expected_failure' key set to true.

This feature is disabled by default in LAUT, to enable set 'blitz_exp_fail_enabled' setting to true first:

.. code-block:: console

   (lӓut-leaf2) set blitz_exp_fail_enabled true
   blitz_exp_fail_enabled - was: False
   now: True
   (lӓut-leaf2)

Once the feature is enabled via setting, for every failed command a prompt appears to ask the user
whether to add the 'expected_failure' key(set to True) to autogenerated blitz action snippet.

A simple example wherein the config 'feature bgp' is expected to fail:

.. code-block:: console

   (lӓut-leaf2) configure feature bgp
   2024-08-03 14:41:22: %LAUT-INFO: +..............................................................................+
   2024-08-03 14:41:22: %LAUT-INFO: :                      Configure 'feature bgp' on 'leaf2'                      :
   2024-08-03 14:41:22: %LAUT-INFO: +..............................................................................+
   
   2024-08-03 14:41:22,040: %UNICON-INFO: +++ leaf2 with via 'a': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   leaf2(config)#feature bgp
   feature bgp
    ^
   % Invalid input detected at '^' marker.
   
   leaf2(config)#end
   leaf2#
   LAUT-ERR: Error configuring 'feature bgp' on 'leaf2'
   ('sub_command failure, patterns matched in the output:', ['^%\\s*[Ii]nvalid (command|input|number|address)'], 'service result', "feature bgp\r\nfeature bgp\r\n ^\r\n% Invalid input detected at '^' marker.\r\n\r\n")
   If this is expected, do you want to add 'expected_failure:True' (y/n): y
   (lӓut-leaf2)
   (lӓut-leaf2) list 1
   configure:
     device: leaf2
     command: feature bgp
     expected_failure: true
   (lӓut-leaf2)

Script abort using continue
---------------------------

When run using ``pyats run job`` or via ``replay`` command, if the action with 'continue' set to false fails,
then the script run will be aborted.

This is supported in LAUT via a prompt requesting the user whether to add 'continue' key(set to False) for
every autogenerated blitz action snippet.

This feature is disabled by default in LAUT, to enable set 'blitz_continue_enabled' setting to true first:

.. code-block:: console

   (lӓut-leaf2) set blitz_continue_enabled true
   blitz_continue_enabled - was: False
   now: True
   (lӓut-leaf2)

A simple example:

.. code-block:: console

   (lӓut-leaf2) configure no logging console
   2024-08-03 14:43:29: %LAUT-INFO: +..............................................................................+
   2024-08-03 14:43:29: %LAUT-INFO: :                  Configure 'no logging console' on 'leaf2'                   :
   2024-08-03 14:43:29: %LAUT-INFO: +..............................................................................+
   
   2024-08-03 14:43:29,824: %UNICON-INFO: +++ leaf2 with via 'a': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   leaf2(config)#no logging console
   leaf2(config)#end
   leaf2#
   Do you want to add 'continue: False' to this action (y/n): y
   (lӓut-leaf2)
   (lӓut-leaf2) list 1
   configure:
     device: leaf2
     command: no logging console
     continue: false
   (lӓut-leaf2)

See below for an aborted ``replay``:

Testcase YAML:

.. code-block:: yaml

   # tc.yaml
   # 03 August 2024
   # LAUT Generated testcase
   tc:
     source:
       pkg: genie.libs.sdk
       class: triggers.blitz.blitz.Blitz
     devices:
       - leaf2
     test_sections:
       - default:
           - execute:
               device: leaf2
               command: show ip route
               continue: false
               include:
                 - 5.5.5.5
           - configure:
               device: leaf2
               command: no logging console

.. code-block:: console

   (lӓut-leaf2) replay pyats/testcases/tc.yaml
   2024-08-03 14:45:53: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-03 14:45:53: %LAUT-INFO: :                         Starting run_genie_sdk "tc"                          :
   2024-08-03 14:45:53: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-03 14:45:53: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-03 14:45:53: %LAUT-INFO: :                          Starting section "default"                          :
   2024-08-03 14:45:53: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-03 14:45:53: %LAUT-INFO: +..............................................................................+
   2024-08-03 14:45:53: %LAUT-INFO: :                      Execute 'show ip route' on 'leaf2'                      :
   2024-08-03 14:45:53: %LAUT-INFO: +..............................................................................+
   
   2024-08-03 14:45:53,352: %UNICON-INFO: +++ leaf2 with via 'a': executing command 'show ip route' +++
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
   O        1.1.1.1 [110/3] via 20.1.1.1, 03:52:05, GigabitEthernet1/0/1
         2.0.0.0/32 is subnetted, 1 subnets
   C        2.2.2.2 is directly connected, Loopback0
         4.0.0.0/32 is subnetted, 1 subnets
   O        4.4.4.4 [110/2] via 20.1.1.1, 5w3d, GigabitEthernet1/0/1
         10.0.0.0/24 is subnetted, 1 subnets
   O        10.10.10.0 [110/2] via 20.1.1.1, 3w5d, GigabitEthernet1/0/1
         20.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
   C        20.1.1.0/24 is directly connected, GigabitEthernet1/0/1
   L        20.1.1.2/32 is directly connected, GigabitEthernet1/0/1
   leaf2#
   2024-08-03 14:45:53: %LAUT-INFO: +..............................................................................+
   2024-08-03 14:45:53: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-08-03 14:45:53: %LAUT-INFO: +..............................................................................+
   2024-08-03 14:45:53: %LAUT-ERR : :                        '5.5.5.5' doesn't match the output                    :
   2024-08-03 14:45:53: %LAUT-INFO: +..............................................................................+
   2024-08-03 14:45:53: %LAUT-INFO: +..............................................................................+
   2024-08-03 14:45:53: %LAUT-ERR : :      'execute' action failed with 'continue' set to False, aborting run      :
   2024-08-03 14:45:53: %LAUT-INFO: +..............................................................................+
   2024-08-03 14:45:53: %LAUT-INFO: +..............................................................................+
   2024-08-03 14:45:53: %LAUT-INFO: :                            run_genie_sdk results                             :
   2024-08-03 14:45:53: %LAUT-INFO: +..............................................................................+
   default:
       execute:
         command: show ip route
         continue: false
         device: leaf2
         include:
         - 5.5.5.5
   2024-08-03 14:45:53: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-03 14:45:53: %LAUT-ERR : :                            run_genie_sdk ABORTED                             :
   2024-08-03 14:45:53: %LAUT-INFO: +------------------------------------------------------------------------------+
   (lӓut-leaf2)

Blitz group
------------

To add testcases under a particular group which could potentially allow users to invoke testcases
belonging to a certain group, the 'group' key is added to every blitz trigger with a list of tags.

This is made available in LAUT via a prompt requesting the list of tags when saving each trigger using the
``save`` command.

This feature is disabled by default in LAUT, to enable set 'blitz_group_enabled' to true first.

See below example:

.. code-block:: console

   (lӓut-leaf2) set blitz_group_enabled true
   blitz_group_enabled - was: False
   now: True
   (lӓut-leaf2) 
   (lӓut-leaf2) list -a
   default:
     - run_genie_sdk:
         tc:
           devices:
             - leaf2
   (lӓut-leaf2) 
   (lӓut-leaf2) save pyats/testcases/sample.yaml
   Do you want to add a blitz group to this testcase? (y/n): y
   Enter blitz group (Multiple groups comma separated): a,b,c
   2024-08-03 14:53:31: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-03 14:53:31: %LAUT-INFO: :            File 'pyats/testcases/sample.yaml' saved successfully             :
   2024-08-03 14:53:31: %LAUT-INFO: +------------------------------------------------------------------------------+
   (lӓut-leaf2) !cat pyats/testcases/sample.yaml
   # sample.yaml
   # 03 August 2024
   # LAUT Generated testcase
   extends:
     - pyats/testcases/tc.yaml
   sample:
     groups:
       - a
       - b
       - c
     source:
       pkg: genie.libs.sdk
       class: triggers.blitz.blitz.Blitz
     devices:
       - leaf2
     test_sections:
       - default:
           - run_genie_sdk:
               tc:
                 devices:
                   - leaf2
   (lӓut-leaf2)
