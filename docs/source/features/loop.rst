Loop
=====

Every LAUT operator command takes an argument to do a basic task.
Loops can help simplify multiple calls for the same command with different arguments.

Basic usage
-----------

'$' is the loop operator. '$' will be prepended over multiple arguments separated by ','
to pass them as a single argument to an operator command.

In an abstract sense,

<command> <arg1>

<command> <arg2>

<command> <arg3>

would become

<command> $<arg1>,<arg2>,<arg3>

Device looping
--------------

By specifying multiple device names(separated by ',') with '$' prepended to them, users can
perform any operator command such as ``execute`` on multiple devices.

A simple example:

.. code-block:: console

   (lӓut-host1) device $host1,host2
   (lӓut-host1,host2)
   (lӓut-host1,host2) execute debug ip mrouting
   2024-08-01 10:23:49: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 10:23:49: %LAUT-INFO: :                            Execute loop, length=2                            :
   2024-08-01 10:23:49: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 10:23:49: %LAUT-INFO: +..............................................................................+
   2024-08-01 10:23:49: %LAUT-INFO: :                    Execute 'debug ip mrouting' on 'host1'                    :
   2024-08-01 10:23:49: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 10:23:49,982: %UNICON-INFO: +++ host1 with via 'a': executing command 'debug ip mrouting' +++
   debug ip mrouting
   IP multicast routing debugging is on
   host1#
   2024-08-01 10:23:50: %LAUT-INFO: +..............................................................................+
   2024-08-01 10:23:50: %LAUT-INFO: :                    Execute 'debug ip mrouting' on 'host2'                    :
   2024-08-01 10:23:50: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 10:23:50,332: %UNICON-INFO: +++ host2 with via 'a': executing command 'debug ip mrouting' +++
   debug ip mrouting
   IP multicast routing debugging is on
   host2#
   (lӓut-host1,host2)

Notice the prompt changes to reflect both the devices to inform users that whatever action performed
will be on both the devices from now onwards. As an LAUT-INFO message, the length of the loop will always be 
printed.

Operator commands that can invoke operation on multiple devices are ``execute``, ``configure``, ``parse`` & ``api``.

Command looping
---------------

One can loop multiple commands using '$' by prepending the dollar sign and separating the commands by ','.

A simple example to loop over 'route' command for both IPv4 and IPv6 address family:

.. code-block:: console

   (lӓut-host1) execute $sh ip route,sh ipv6 route
   2024-08-01 11:42:51: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 11:42:51: %LAUT-INFO: :                            Execute loop, length=2                            :
   2024-08-01 11:42:51: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 11:42:51: %LAUT-INFO: +..............................................................................+
   2024-08-01 11:42:51: %LAUT-INFO: :                      Execute 'show ip route' on 'host1'                      :
   2024-08-01 11:42:51: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 11:42:51,352: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ip route' +++
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
   host1#
   2024-08-01 11:42:51: %LAUT-INFO: +..............................................................................+
   2024-08-01 11:42:51: %LAUT-INFO: :                     Execute 'show ipv6 route' on 'host1'                     :
   2024-08-01 11:42:51: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 11:42:51,600: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ipv6 route' +++
   show ipv6 route
   host1#
   (lӓut-host1)

To simplify this further, notice that the difference between the 2 commands were only at the keywords 'ip' and 'ipv6'.
Hence, the command could be reduced to ``execute show $(ip,ipv6) route`` with parenthesis emphasizing the start and end
of the loop argument, with the values outside the parenthesis being the same across all loop iterations.

As seen below, the minimalized command produces the same operation seen above:

.. code-block:: console

   (lӓut-host1) show $(ip,ipv6) route
   2024-08-01 11:54:29: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 11:54:29: %LAUT-INFO: :                            Execute loop, length=2                            :
   2024-08-01 11:54:29: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 11:54:29: %LAUT-INFO: +..............................................................................+
   2024-08-01 11:54:29: %LAUT-INFO: :                      Execute 'show ip route' on 'host1'                      :
   2024-08-01 11:54:29: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 11:54:29,404: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ip route' +++
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
   host1#
   2024-08-01 11:54:29: %LAUT-INFO: +..............................................................................+
   2024-08-01 11:54:29: %LAUT-INFO: :                     Execute 'show ipv6 route' on 'host1'                     :
   2024-08-01 11:54:29: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 11:54:29,695: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ipv6 route' +++
   show ipv6 route
   host1#
   (lӓut-host1)

Consider a more complex example wherein we have 6 commands:

   * show ip mroute
   * show ip mfib
   * show ip mrib route
   * show ipv6 mroute
   * show ipv6 mfib
   * show ipv6 mrib route

All these commands if needed to be executed would take 6 times to type, but considering the similarities
between all of them, we can create loops as shown:

Let's separate the IPv4 from IPv6 and try looping the IPv4 alone with IPv6 being invoked separately.

   * $show ip mroute,show ip mfib,show ip mrib route
   * $show ipv6 mroute,show ipv6 mfib,show ipv6 mrib route

Let's minimalize the 2 commands above using parenthesis:

   * show ip $(mroute,mfib,mrib route)
   * show ipv6 $(mroute,mfib,mrib route)

Since we have written the '$' notation, the entire loop argument could be seen identical to a string to further
consider the possibility of multiple loops. Let XXX represent $(mroute,mfib,mrib route). Now, the 2 commands become:

   * show ip XXX
   * show ipv6 XXX

Using normal loop principles from earlier, we could create a loop here as shown:

   * show $(ip,ipv6) XXX

Finally, substituting XXX brings us to a double loop:

   * show $(ip,ipv6) $(mroute,mfib,mrib route)

The result of 2 '$' and 2 hence loops would be the 'product' akin to a cartesian product with (A,B) * (C,D) -> AC, AD, BC, BD.
The length of the loop would be the product of number of items in each loop; with the earlier example having 2 * 3 = 6 iterations
which was the total number of commands we tried to invoke in the first place.

.. code-block:: console

   (lӓut-host1) show $(ip,ipv6) $(mroute,mfib,mrib route)
   2024-08-01 12:02:58: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 12:02:58: %LAUT-INFO: :                            Execute loop, length=6                            :
   2024-08-01 12:02:58: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 12:02:58: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:02:58: %LAUT-INFO: :                     Execute 'show ip mroute' on 'host1'                      :
   2024-08-01 12:02:58: %LAUT-INFO: +..............................................................................+
   <TRUNCATED>
   2024-08-01 12:02:59: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:02:59: %LAUT-INFO: :                      Execute 'show ip mfib' on 'host1'                       :
   2024-08-01 12:02:59: %LAUT-INFO: +..............................................................................+
   <TRUNCATED>
   2024-08-01 12:02:59: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:02:59: %LAUT-INFO: :                   Execute 'show ip mrib route' on 'host1'                    :
   2024-08-01 12:02:59: %LAUT-INFO: +..............................................................................+
   <TRUNCATED>
   2024-08-01 12:02:59: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:02:59: %LAUT-INFO: :                    Execute 'show ipv6 mroute' on 'host1'                     :
   2024-08-01 12:02:59: %LAUT-INFO: +..............................................................................+
   <TRUNCATED>
   2024-08-01 12:03:00: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:03:00: %LAUT-INFO: :                     Execute 'show ipv6 mfib' on 'host1'                      :
   2024-08-01 12:03:00: %LAUT-INFO: +..............................................................................+
   <TRUNCATED>
   2024-08-01 12:03:00: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:03:00: %LAUT-INFO: :                  Execute 'show ipv6 mrib route' on 'host1'                   :
   2024-08-01 12:03:00: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 12:03:00,559: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ipv6 mrib route' +++
   show ipv6 mrib route
   No matching routes in MRIB route-DB
   
   host1#
   (lӓut-host1)

As seen above, the command with 2 '$' indeed invoked all the 6 commands we needed.

The same product rule applies when there is both command looping as well as device looping.

Let's do device looping across 2 devices with

   * device $host1,host2

Then if we try 3 commands with another '$' for command loop:

   * show ip $(mroute,mfib,mrib route)

The result would eventually be a loop of length (2 * 3 = 6) with the 3 commands first applied
on device 'host1' and then on 'host2':

.. code-block:: console

   (lӓut-host1) device $host1,host2
   (lӓut-host1,host2) show ip $(mroute,mfib,mrib route)
   2024-08-01 12:10:43: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 12:10:43: %LAUT-INFO: :                            Execute loop, length=6                            :
   2024-08-01 12:10:43: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 12:10:43: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:10:43: %LAUT-INFO: :                     Execute 'show ip mroute' on 'host1'                      :
   2024-08-01 12:10:43: %LAUT-INFO: +..............................................................................+
   <TRUNCATED>
   2024-08-01 12:10:43: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:10:43: %LAUT-INFO: :                      Execute 'show ip mfib' on 'host1'                       :
   2024-08-01 12:10:43: %LAUT-INFO: +..............................................................................+
   <TRUNCATED>
   2024-08-01 12:10:44: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:10:44: %LAUT-INFO: :                   Execute 'show ip mrib route' on 'host1'                    :
   2024-08-01 12:10:44: %LAUT-INFO: +..............................................................................+
   <TRUNCATED>
   2024-08-01 12:10:44: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:10:44: %LAUT-INFO: :                     Execute 'show ip mroute' on 'host2'                      :
   2024-08-01 12:10:44: %LAUT-INFO: +..............................................................................+
   <TRUNCATED>
   2024-08-01 12:10:44: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:10:44: %LAUT-INFO: :                      Execute 'show ip mfib' on 'host2'                       :
   2024-08-01 12:10:44: %LAUT-INFO: +..............................................................................+
   <TRUNCATED>
   2024-08-01 12:10:45: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:10:45: %LAUT-INFO: :                   Execute 'show ip mrib route' on 'host2'                    :
   2024-08-01 12:10:45: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 12:10:45,150: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip mrib route' +++
   show ip mrib route
   No matching routes in MRIB route-DB
   
   host2#
   (lӓut-host1,host2)

What if we want to loop different commands for both the devices as follows:

   * host1: show ip route 1.1.1.1
   * host2: show ip route 2.2.2.2

We need to loop but modify the loop values of device loop with some values in command. To modify
existing loop iteration values, we use $[] instead of $():

   * host1,host2: $[show ip route 1.1.1.1,show ip route 2.2.2.2]

This time we are not creating a loop with '1.1.1.1' and '2.2.2.2' but rather saying that the loop (host1,host2)
should invoke two different commands. Minimalizing the command, we get

   * host1,host2: show ip route $[1.1.1.1,2.2.2.2]

.. code-block:: console

   (lӓut-host1,host2) show ip route $[1.1.1.1,2.2.2.2]
   2024-08-01 12:26:39: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 12:26:39: %LAUT-INFO: :                            Execute loop, length=2                            :
   2024-08-01 12:26:39: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 12:26:39: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:26:39: %LAUT-INFO: :                  Execute 'show ip route 1.1.1.1' on 'host1'                  :
   2024-08-01 12:26:39: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 12:26:39,793: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ip route 1.1.1.1' +++
   show ip route 1.1.1.1
   Routing entry for 1.1.1.1/32
     Known via "connected", distance 0, metric 0 (connected, via interface)
     Routing Descriptor Blocks:
     * directly connected, via Loopback0
         Route metric is 0, traffic share count is 1
   host1#
   2024-08-01 12:26:39: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:26:39: %LAUT-INFO: :                  Execute 'show ip route 2.2.2.2' on 'host2'                  :
   2024-08-01 12:26:39: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 12:26:40,059: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip route 2.2.2.2' +++
   show ip route 2.2.2.2
   Routing entry for 2.2.2.2/32
     Known via "connected", distance 0, metric 0 (connected, via interface)
     Routing Descriptor Blocks:
     * directly connected, via Loopback1
         Route metric is 0, traffic share count is 1
   host2#
   (lӓut-host1,host2)

One can also specify multiple loops and in case want to modify iteration values of a particular loop just mention
the 'back reference' number of the loop you want to modify in the format $<back_ref_no>[arg1,arg2].

As far as the autogenerated blitz snippets are concerned, each loop argument will create a separate
blitz action snippet. Take a look at example below:

.. code-block:: console

   (lӓut-host1) show $(ip,ipv6) route -i
   2024-08-01 12:38:52: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 12:38:52: %LAUT-INFO: :                            Execute loop, length=2                            :
   2024-08-01 12:38:52: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 12:38:52: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:38:52: %LAUT-INFO: :                      Execute 'show ip route' on 'host1'                      :
   2024-08-01 12:38:52: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 12:38:52,693: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ip route' +++
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
   host1#
   2024-08-01 12:38:52: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:38:52: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-08-01 12:38:52: %LAUT-INFO: +..............................................................................+
   Enter pattern to INCLUDE (Press enter for multiple patterns): 1.1.1.1
   2024-08-01 12:38:54: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:38:54: %LAUT-INFO: :                     Execute 'show ipv6 route' on 'host1'                     :
   2024-08-01 12:38:54: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 12:38:54,832: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ipv6 route' +++
   show ipv6 route
   host1#
   2024-08-01 12:38:54: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:38:54: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-08-01 12:38:54: %LAUT-INFO: +..............................................................................+
   Enter pattern to INCLUDE (Press enter for multiple patterns):
   (INCLUDE)>
   (lӓut-host1) list 2
   execute:
     device: host1
     command: show ip route
     include:
       - 1.1.1.1
   execute:
     device: host1
     command: show ipv6 route
   (lӓut-host1)

All *include*, *exclude* entries will be separate between the 2 commands as well.

Api parameter looping
---------------------

One can also loop in ``api`` parameters as shown below with the parameter 'vrf' being invoked
with both 'red' and 'blue' via '$(red,blue)'

.. code-block:: console

   (lӓut-host1) api _get_vrf_interface
   Gets the subinterfaces for vrf
   
   Args:
       device ('obj'): device to run on
       vrf ('str'): vrf to search under
   
   Returns:
       interfaces('list'): List of interfaces under specified vrf
       None
   
   Raises:
       None
   
   device: [<Device host1 at 0x7fc37ff03520>]
   vrf: $(red,blue)
   2024-08-01 12:36:57: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 12:36:57: %LAUT-INFO: :                              Api loop, length=2                              :
   2024-08-01 12:36:57: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-01 12:36:57: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:36:57: %LAUT-INFO: :                   Api 'get_vrf_interface' with parameters:                   :
   2024-08-01 12:36:57: %LAUT-INFO: :                                device: host1                                 :
   2024-08-01 12:36:57: %LAUT-INFO: :                                  vrf: 'red'                                  :
   2024-08-01 12:36:57: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 12:36:59,615: %UNICON-INFO: +++ host1 with via 'a': executing command 'show vrf red' +++
   show vrf red
     Name                             Default RD            Protocols   Interfaces
     red                              <not set>                         Lo99
   host1#
   2024-08-01 12:36:59: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:36:59: %LAUT-INFO: :                                 Api output:                                  :
   2024-08-01 12:36:59: %LAUT-INFO: +..............................................................................+
                                                                     ['Loopback99']
   2024-08-01 12:36:59: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:36:59: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:36:59: %LAUT-INFO: :                   Api 'get_vrf_interface' with parameters:                   :
   2024-08-01 12:36:59: %LAUT-INFO: :                                device: host1                                 :
   2024-08-01 12:36:59: %LAUT-INFO: :                                 vrf: 'blue'                                  :
   2024-08-01 12:36:59: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 12:37:00,035: %UNICON-INFO: +++ host1 with via 'a': executing command 'show vrf blue' +++
   show vrf blue
   % No VRF named blue
   host1#
   2024-08-01 12:37:00: %LAUT-INFO: +..............................................................................+
   2024-08-01 12:37:00: %LAUT-INFO: :                                 Api output:                                  :
   2024-08-01 12:37:00: %LAUT-INFO: +..............................................................................+
                                                                          None
   2024-08-01 12:37:00: %LAUT-INFO: +..............................................................................+
   (lӓut-host1)
   (lӓut-host1) list 2
   api:
     function: get_vrf_interface
     arguments:
       device: host1
       vrf: red
   api:
     function: get_vrf_interface
     arguments:
       device: host1
       vrf: blue

If any of the API parameter happens to be a loop, then all the parameters are copied to each of the loop
iteration much in the same way minimalization works for command arguments. To illustrate, see the following
analysis for the api loop seen earlier:

   * get_vrf_interface( host1, $(red,blue) )

Removing minimalization in the API arguments:

   * get_vrf_interface( $(host1,red) , (host1,blue) )

api $ is equal to invoking api multiple times:

   * get_vrf_interface( host1,red  )
   * get_vrf_interface( host1,blue )

Here is an example highlighting how multiple loops work in ``api``. Our task at hand is to configure 4 different
CLI's via the api 'configure_ip_igmp_join_group_source' like this:

   * configure_ip_igmp_join_group_source(leaf2, Loopback0, 225.1.1.1, 30.0.0.1)
   * configure_ip_igmp_join_group_source(leaf2, Loopback0, 225.1.1.1, 30.0.0.2)
   * configure_ip_igmp_join_group_source(leaf2, Loopback0, 226.1.1.1, 30.0.0.1)
   * configure_ip_igmp_join_group_source(leaf2, Loopback0, 226.1.1.1, 30.0.0.2)

Making the addresses '225.1.1.1' & '226.1.1.1' into a loop:

   * configure_ip_igmp_join_group_source( leaf2, Loopback0, $(225.1.1.1,226.1.1.1), 30.0.0.1 )
   * configure_ip_igmp_join_group_source( leaf2, Loopback0, $(225.1.1.1,226.1.1.1), 30.0.0.2 )

We could further make the addresses '30.0.0.1' & '30.0.0.2' also into another loop. The whole point of double
loops could be easily conceived in this case if you can consider the earlier loop to be another string just like
'leaf2' or 'Loopback0'. Let us keep $(225.1.1.1,226.1.1.1) = XXX to avoid confusion.

   * configure_ip_igmp_join_group_source( leaf2, Loopback0, XXX, 30.0.0.1 )
   * configure_ip_igmp_join_group_source( leaf2, Loopback0, XXX, 30.0.0.2 )

Now, applying loop principles we get:

   * configure_ip_igmp_join_group_source(leaf2, Loopback0, XXX, $(30.0.0.1,30.0.0.2) )

As seen above, it is easier to conceive double or multiple loops this way. Hence finally, we arrive at the
whole loop sequence by substituting XXX:

   * configure_ip_igmp_join_group_source( leaf2, Loopback0, $(225.1.1.1,226.1.1.1), $(30.0.0.1,30.0.0.2) )

Invoking the api on LAUT now:

.. code-block:: console

   (lӓut-leaf2) api multicast configure configure_ip_igmp_join_group_source
   Configures ip igmp join-group to an vlan interface
   Example : ip igmp join-group 239.100.100.101 source 4.4.4.4
   
   Args:
       device ('obj'): device to use
       interface ('str'): interface or Vlan number (Eg. ten1/0/1 or vlan 10)
       group_address ('str'): IP group addres
       source_address ('str', optional): IP source address
   
   Returns:
       None
   
   Raises:
       SubCommandFailure
   
   device: [<Device leaf2 at 0x7f43ae85a640>]
   interface: Loopback0
   group_address: $225.1.1.1,226.1.1.1
   source_address(''): $30.0.0.1,30.0.0.2
   2024-08-08 09:05:44: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-08 09:05:44: %LAUT-INFO: :                              Api loop, length=4                              :
   2024-08-08 09:05:44: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-08-08 09:05:44: %LAUT-INFO: +..............................................................................+
   2024-08-08 09:05:44: %LAUT-INFO: :          Api 'configure_ip_igmp_join_group_source' with parameters:          :
   2024-08-08 09:05:44: %LAUT-INFO: :                                device: leaf2                                 :
   2024-08-08 09:05:44: %LAUT-INFO: :                            interface: 'Loopback0'                            :
   2024-08-08 09:05:44: %LAUT-INFO: :                          group_address: '225.1.1.1'                          :
   2024-08-08 09:05:44: %LAUT-INFO: :                          source_address: '30.0.0.1'                          :
   2024-08-08 09:05:44: %LAUT-INFO: +..............................................................................+
   INFO:genie.libs.sdk.apis.iosxe.multicast.configure:Configuring ip igmp join-group on leaf2
   
   2024-08-08 09:05:44,463: %UNICON-INFO: +++ leaf2 with via 'a': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   leaf2(config)#interface Loopback0
   leaf2(config-if)#ip igmp join-group 225.1.1.1 source 30.0.0.1
   leaf2(config-if)#end
   leaf2#
   2024-08-08 09:05:44: %LAUT-INFO: +..............................................................................+
   2024-08-08 09:05:44: %LAUT-INFO: :                                 Api output:                                  :
   2024-08-08 09:05:44: %LAUT-INFO: +..............................................................................+
                                                                          None
   2024-08-08 09:05:44: %LAUT-INFO: +..............................................................................+
   2024-08-08 09:05:44: %LAUT-INFO: +..............................................................................+
   2024-08-08 09:05:44: %LAUT-INFO: :          Api 'configure_ip_igmp_join_group_source' with parameters:          :
   2024-08-08 09:05:44: %LAUT-INFO: :                                device: leaf2                                 :
   2024-08-08 09:05:44: %LAUT-INFO: :                            interface: 'Loopback0'                            :
   2024-08-08 09:05:44: %LAUT-INFO: :                          group_address: '225.1.1.1'                          :
   2024-08-08 09:05:44: %LAUT-INFO: :                          source_address: '30.0.0.2'                          :
   2024-08-08 09:05:44: %LAUT-INFO: +..............................................................................+
   INFO:genie.libs.sdk.apis.iosxe.multicast.configure:Configuring ip igmp join-group on leaf2
   
   2024-08-08 09:05:44,968: %UNICON-INFO: +++ leaf2 with via 'a': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   leaf2(config)#interface Loopback0
   leaf2(config-if)#ip igmp join-group 225.1.1.1 source 30.0.0.2
   leaf2(config-if)#end
   leaf2#
   2024-08-08 09:05:45: %LAUT-INFO: +..............................................................................+
   2024-08-08 09:05:45: %LAUT-INFO: :                                 Api output:                                  :
   2024-08-08 09:05:45: %LAUT-INFO: +..............................................................................+
                                                                          None
   2024-08-08 09:05:45: %LAUT-INFO: +..............................................................................+
   2024-08-08 09:05:45: %LAUT-INFO: +..............................................................................+
   2024-08-08 09:05:45: %LAUT-INFO: :          Api 'configure_ip_igmp_join_group_source' with parameters:          :
   2024-08-08 09:05:45: %LAUT-INFO: :                                device: leaf2                                 :
   2024-08-08 09:05:45: %LAUT-INFO: :                            interface: 'Loopback0'                            :
   2024-08-08 09:05:45: %LAUT-INFO: :                          group_address: '226.1.1.1'                          :
   2024-08-08 09:05:45: %LAUT-INFO: :                          source_address: '30.0.0.1'                          :
   2024-08-08 09:05:45: %LAUT-INFO: +..............................................................................+
   INFO:genie.libs.sdk.apis.iosxe.multicast.configure:Configuring ip igmp join-group on leaf2
   
   2024-08-08 09:05:45,468: %UNICON-INFO: +++ leaf2 with via 'a': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   leaf2(config)#interface Loopback0
   leaf2(config-if)#ip igmp join-group 226.1.1.1 source 30.0.0.1
   leaf2(config-if)#end
   leaf2#
   2024-08-08 09:05:45: %LAUT-INFO: +..............................................................................+
   2024-08-08 09:05:45: %LAUT-INFO: :                                 Api output:                                  :
   2024-08-08 09:05:45: %LAUT-INFO: +..............................................................................+
                                                                          None
   2024-08-08 09:05:45: %LAUT-INFO: +..............................................................................+
   2024-08-08 09:05:45: %LAUT-INFO: +..............................................................................+
   2024-08-08 09:05:45: %LAUT-INFO: :          Api 'configure_ip_igmp_join_group_source' with parameters:          :
   2024-08-08 09:05:45: %LAUT-INFO: :                                device: leaf2                                 :
   2024-08-08 09:05:45: %LAUT-INFO: :                            interface: 'Loopback0'                            :
   2024-08-08 09:05:45: %LAUT-INFO: :                          group_address: '226.1.1.1'                          :
   2024-08-08 09:05:45: %LAUT-INFO: :                          source_address: '30.0.0.2'                          :
   2024-08-08 09:05:45: %LAUT-INFO: +..............................................................................+
   INFO:genie.libs.sdk.apis.iosxe.multicast.configure:Configuring ip igmp join-group on leaf2
   
   2024-08-08 09:05:45,974: %UNICON-INFO: +++ leaf2 with via 'a': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   leaf2(config)#interface Loopback0
   leaf2(config-if)#ip igmp join-group 226.1.1.1 source 30.0.0.2
   leaf2(config-if)#end
   leaf2#
   2024-08-08 09:05:46: %LAUT-INFO: +..............................................................................+
   2024-08-08 09:05:46: %LAUT-INFO: :                                 Api output:                                  :
   2024-08-08 09:05:46: %LAUT-INFO: +..............................................................................+
                                                                          None
   2024-08-08 09:05:46: %LAUT-INFO: +..............................................................................+
   (lӓut-leaf2)

The autogenerated blitz *'api'* action snippets are shown below:

.. code-block:: console

   (lӓut-leaf2) list 4
   api:
     function: configure_ip_igmp_join_group_source
     arguments:
       device: leaf2
       interface: Loopback0
       group_address: 225.1.1.1
       source_address: 30.0.0.1
   api:
     function: configure_ip_igmp_join_group_source
     arguments:
       device: leaf2
       interface: Loopback0
       group_address: 225.1.1.1
       source_address: 30.0.0.2
   api:
     function: configure_ip_igmp_join_group_source
     arguments:
       device: leaf2
       interface: Loopback0
       group_address: 226.1.1.1
       source_address: 30.0.0.1
   api:
     function: configure_ip_igmp_join_group_source
     arguments:
       device: leaf2
       interface: Loopback0
       group_address: 226.1.1.1
       source_address: 30.0.0.2

.. note::

   If one of the commands failed in a loop, LAUT behaves as if that loop iteration wasn't a part of it
   in the first place. This would in turn make other loop iterations autogenerate blitz action snippets
   and the failed loop iteration would not autogenerate any snippet at all.
