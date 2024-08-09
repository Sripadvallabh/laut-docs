Console logs
============

LAUT can produce one log file for each device in a testbed that contains
all the actions performed on that device.

Basic usage
-----------

Set 'console_logs_dir' setting to point to the directory where the logs are to be
dumped. See below for an example wherein two devices 'leaf2' and 'spine' had their
log files 'leaf2.log' and 'spine.log' created under the given directory:

.. code-block:: console

   (lӓut-leaf2) set console_logs_dir ../../alternate_msdp/
   
   2024-08-06 16:13:22,419: %UNICON-INFO: +++ logfile: /nobackup/araradha/alternate_msdp/leaf2.log +++
   
   2024-08-06 16:13:22,447: %UNICON-INFO: +++ logfile: /nobackup/araradha/alternate_msdp/spine.log +++
   console_logs_dir - was: None
   now: '/nobackup/araradha/alternate_msdp'
   (lӓut-leaf2)

Please note the 'UNICON-INFO' log message printed above suggesting a successful log file linked with the
device using unicon API log_file().
Starting from now, any operation on the device via unicon API's such as execute or configure
will always write the buffer to the log file.

Now, let us try 'show ip route' on device 'leaf2':

.. code-block:: console

   (lӓut-leaf2) show ip route
   2024-08-06 16:14:35: %LAUT-INFO: +..............................................................................+
   2024-08-06 16:14:35: %LAUT-INFO: :                      Execute 'show ip route' on 'leaf2'                      :
   2024-08-06 16:14:35: %LAUT-INFO: +..............................................................................+
   
   2024-08-06 16:14:35,547: %UNICON-INFO: +++ leaf2 with via 'a': executing command 'show ip route' +++
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
   O        1.1.1.1 [110/3] via 20.1.1.1, 00:15:33, GigabitEthernet1/0/1
         2.0.0.0/32 is subnetted, 1 subnets
   C        2.2.2.2 is directly connected, Loopback0
         4.0.0.0/32 is subnetted, 1 subnets
   O        4.4.4.4 [110/2] via 20.1.1.1, 3d00h, GigabitEthernet1/0/1
         10.0.0.0/24 is subnetted, 1 subnets
   O        10.10.10.0 [110/2] via 20.1.1.1, 3d00h, GigabitEthernet1/0/1
         20.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
   C        20.1.1.0/24 is directly connected, GigabitEthernet1/0/1
   L        20.1.1.2/32 is directly connected, GigabitEthernet1/0/1
   leaf2#

If we observe the log file, it contains the 'show ip route' command output we tried earlier:

.. code-block:: console

   (lӓut-leaf2) shell cat ../../alternate_msdp/leaf2.log
   [2024-08-06 16:14:35,547] +++ leaf2 with via 'a': executing command 'show ip route' +++
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
   O        1.1.1.1 [110/3] via 20.1.1.1, 00:15:33, GigabitEthernet1/0/1
         2.0.0.0/32 is subnetted, 1 subnets
   C        2.2.2.2 is directly connected, Loopback0
         4.0.0.0/32 is subnetted, 1 subnets
   O        4.4.4.4 [110/2] via 20.1.1.1, 3d00h, GigabitEthernet1/0/1
         10.0.0.0/24 is subnetted, 1 subnets
   O        10.10.10.0 [110/2] via 20.1.1.1, 3d00h, GigabitEthernet1/0/1
         20.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
   C        20.1.1.0/24 is directly connected, GigabitEthernet1/0/1
   L        20.1.1.2/32 is directly connected, GigabitEthernet1/0/1
   leaf2#
   (lӓut-leaf2)

If we try a configuration 'no logging console' on the same device, it too appears
in the log file as seen below:

.. code-block:: console

   (lӓut-leaf2) configure no logging console
   2024-08-06 16:16:53: %LAUT-INFO: +..............................................................................+
   2024-08-06 16:16:53: %LAUT-INFO: :                  Configure 'no logging console' on 'leaf2'                   :
   2024-08-06 16:16:53: %LAUT-INFO: +..............................................................................+
   
   2024-08-06 16:16:53,242: %UNICON-INFO: +++ leaf2 with via 'a': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   leaf2(config)#no logging console
   leaf2(config)#end
   leaf2#
   (lӓut-leaf2) 
   (lӓut-leaf2) shell cat ../../alternate_msdp/leaf2.log
   [2024-08-06 16:14:35,547] +++ leaf2 with via 'a': executing command 'show ip route' +++
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
   O        1.1.1.1 [110/3] via 20.1.1.1, 00:15:33, GigabitEthernet1/0/1
         2.0.0.0/32 is subnetted, 1 subnets
   C        2.2.2.2 is directly connected, Loopback0
         4.0.0.0/32 is subnetted, 1 subnets
   O        4.4.4.4 [110/2] via 20.1.1.1, 3d00h, GigabitEthernet1/0/1
         10.0.0.0/24 is subnetted, 1 subnets
   O        10.10.10.0 [110/2] via 20.1.1.1, 3d00h, GigabitEthernet1/0/1
         20.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
   C        20.1.1.0/24 is directly connected, GigabitEthernet1/0/1
   L        20.1.1.2/32 is directly connected, GigabitEthernet1/0/1
   leaf2#
   [2024-08-06 16:16:53,242] +++ leaf2 with via 'a': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   leaf2(config)#no logging console
   leaf2(config)#end
   leaf2#
   (lӓut-leaf2)
