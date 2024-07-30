replay
======

Runs LAUT generated blitz testcases and autogenerates pyATS blitz *'run_genie_sdk'*
action snippet.

Why replay
----------

Repeated tasks are bound to occur in any network/unit-testing workflows. Capturing
these tasks under one blitz testcase and re-running inside other blitz testcases
creates a modularized workflow thereby maximizing efficiency; and is analogous to functions in
programming languages.

Basic usage
-----------

A sample LAUT generated blitz testcase YAML is provided below:

.. code-block:: yaml

   # sample.yaml
   # 29 July 2024
   # LAUT Generated testcase
   sample:
     source:
       pkg: genie.libs.sdk
       class: triggers.blitz.blitz.Blitz
     devices:
       - host1
     test_sections:
       - default:
           - configure:
               device: host1
               command: no logging console
       - new_1:
           - execute:
               device: host1
               command: show ip route
               include:
                 - 1.1.1.1
       - new_2:
           - sleep:
               sleep_time: 5

Invoking ``replay`` with this testcase YAML as argument would run this testcase as shown below:

.. code-block:: console

   (lӓut-host1) replay pyats/testcases/sample.yaml
   2024-07-29 21:03:53: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:03:53: %LAUT-INFO: :                       Starting run_genie_sdk "sample"                        :
   2024-07-29 21:03:53: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:03:53: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:03:53: %LAUT-INFO: :                          Starting section "default"                          :
   2024-07-29 21:03:53: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:03:53: %LAUT-INFO: +..............................................................................+
   2024-07-29 21:03:53: %LAUT-INFO: :                  Configure 'no logging console' on 'host1'                   :
   2024-07-29 21:03:53: %LAUT-INFO: +..............................................................................+
   
   2024-07-29 21:03:53,775: %UNICON-INFO: +++ host1 with via 'a': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   host1(config)#no logging console
   host1(config)#end
   host1#
   2024-07-29 21:03:54: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:03:54: %LAUT-INFO: :                           Starting section "new_1"                           :
   2024-07-29 21:03:54: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:03:54: %LAUT-INFO: +..............................................................................+
   2024-07-29 21:03:54: %LAUT-INFO: :                      Execute 'show ip route' on 'host1'                      :
   2024-07-29 21:03:54: %LAUT-INFO: +..............................................................................+
   
   2024-07-29 21:03:54,136: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ip route' +++
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
   C        1.1.1.1 is directly connected, Loopback30
   host1#
   2024-07-29 21:03:54: %LAUT-INFO: +..............................................................................+
   2024-07-29 21:03:54: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-07-29 21:03:54: %LAUT-INFO: +..............................................................................+
   2024-07-29 21:03:54: %LAUT-INFO: :                                '1.1.1.1' matches                             :
   2024-07-29 21:03:54: %LAUT-INFO: +..............................................................................+
   2024-07-29 21:03:54: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:03:54: %LAUT-INFO: :                           Starting section "new_2"                           :
   2024-07-29 21:03:54: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:03:59: %LAUT-INFO: +..............................................................................+
   2024-07-29 21:03:59: %LAUT-INFO: :                            run_genie_sdk results                             :
   2024-07-29 21:03:59: %LAUT-INFO: +..............................................................................+
   default:
   new_1:
   new_2:
   2024-07-29 21:03:59: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:03:59: %LAUT-INFO: :                             run_genie_sdk PASSED                             :
   2024-07-29 21:03:59: %LAUT-INFO: +------------------------------------------------------------------------------+
   (lӓut-host1)

Once the run is complete, results will be printed along with the final 'PASSED/FAILED' banner. The entire
run looks very much alike to that done via ``pyats run job``.

``replay`` autogenerates *'run_genie_sdk'* blitz action snippet as shown below:

.. code-block:: console

   (lӓut-host1) list 1
   run_genie_sdk:
     sample:
       devices: host1
   (lӓut-host1)

And when the current testcase is saved, the path to all the testcases ran via ``replay`` command will be added
to blitz *'extends'* key as shown below:

.. code-block:: yaml

   # sample.yaml
   # 29 July 2024
   # LAUT Generated testcase
   extends:
     - pyats/testcases/sample.yaml
   sample:
     source:
       pkg: genie.libs.sdk
       class: triggers.blitz.blitz.Blitz
     devices:
       - host1
     test_sections:
       - default:
           - run_genie_sdk:
               sample:
                 devices: host1

Results
--------

Results are printed at the end of the run. When all actions under a given test section passes, the
test section name is printed in green; if a given test section fails the actions under it are printed
in corresponding green/red based on their pass/fail along with the test section name itself being printed in red.

See example below for the same testcase being invoked via ``replay``; this time the ``execute`` action should fail
due to the interface being *shut*:

.. code-block:: console

   (lӓut-host1) configure int Loopback0 + shutdown
   2024-07-29 21:17:23: %LAUT-INFO: +..............................................................................+
   2024-07-29 21:17:23: %LAUT-INFO: :             Configure ['int Loopback0 ', ' shutdown'] on 'host1'             :
   2024-07-29 21:17:23: %LAUT-INFO: +..............................................................................+
   
   2024-07-29 21:17:23,990: %UNICON-INFO: +++ host1 with via 'a': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   host1(config)#int Loopback0
   host1(config-if)# shutdown
   host1(config-if)#end
   host1#
   (lӓut-host1) replay pyats/testcases/sample.yaml
   2024-07-29 21:17:28: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:17:28: %LAUT-INFO: :                       Starting run_genie_sdk "sample"                        :
   2024-07-29 21:17:28: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:17:28: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:17:28: %LAUT-INFO: :                          Starting section "default"                          :
   2024-07-29 21:17:28: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:17:28: %LAUT-INFO: +..............................................................................+
   2024-07-29 21:17:28: %LAUT-INFO: :                  Configure 'no logging console' on 'host1'                   :
   2024-07-29 21:17:28: %LAUT-INFO: +..............................................................................+
   
   2024-07-29 21:17:28,753: %UNICON-INFO: +++ host1 with via 'a': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   host1(config)#no logging console
   host1(config)#end
   host1#
   2024-07-29 21:17:28: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:17:28: %LAUT-INFO: :                           Starting section "new_1"                           :
   2024-07-29 21:17:28: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:17:28: %LAUT-INFO: +..............................................................................+
   2024-07-29 21:17:28: %LAUT-INFO: :                      Execute 'show ip route' on 'host1'                      :
   2024-07-29 21:17:28: %LAUT-INFO: +..............................................................................+
   
   2024-07-29 21:17:29,114: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ip route' +++
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
   
   host1#
   2024-07-29 21:17:29: %LAUT-INFO: +..............................................................................+
   2024-07-29 21:17:29: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-07-29 21:17:29: %LAUT-INFO: +..............................................................................+
   2024-07-29 21:17:29: %LAUT-ERR : :                        '1.1.1.1' doesn't match the output                    :
   2024-07-29 21:17:29: %LAUT-INFO: +..............................................................................+
   2024-07-29 21:17:29: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:17:29: %LAUT-INFO: :                           Starting section "new_2"                           :
   2024-07-29 21:17:29: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:17:34: %LAUT-INFO: +..............................................................................+
   2024-07-29 21:17:34: %LAUT-INFO: :                            run_genie_sdk results                             :
   2024-07-29 21:17:34: %LAUT-INFO: +..............................................................................+
   default:
   new_1:
       execute:
         command: show ip route
         device: host1
         include:
         - 1.1.1.1
   new_2:
   2024-07-29 21:17:34: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-29 21:17:34: %LAUT-ERR : :                             run_genie_sdk FAILED                             :
   2024-07-29 21:17:34: %LAUT-INFO: +------------------------------------------------------------------------------+
   (lӓut-host1)

As seen above, test section 'new_1' failed to verify route to '1.1.1.1' and hence the *'execute'* action under
the test section 'new_1' was printed for user to check upon; with the other 2 sections still being passed and with
just their names printed in green. Overall, the run had FAILED and this is also printed in the end.
