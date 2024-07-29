execute
=======

Executes a command on the device loaded in LAUT shell and can autogenerate
pyATS blitz *'execute'* action snippet.

Basic usage
------------

An example of ``execute show ip route`` on device 'host2' is shown below:

.. code-block:: console

   (lӓut-host2) execute show ip route
   2024-07-26 15:40:42: %LAUT-INFO: +..............................................................................+
   2024-07-26 15:40:42: %LAUT-INFO: :                      Execute 'show ip route' on 'host2'                      :
   2024-07-26 15:40:42: %LAUT-INFO: +..............................................................................+
   
   2024-07-26 15:40:42,819: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip route' +++
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
   (lӓut-host2)

The output from the command execution will be printed on the terminal screen. LAUT does not
autogenerate blitz *'execute'* action snippet for executing a command; rather it autogenerates only
when a ``execute`` command is called with the arguments to add *include* or *exclude* entries
as seen below.

.. note::

   There is a shortcut command 'show' which is essentially the actual command ``execute show``
   intended for easiness to invoke show commands. Read more on shortcuts :ref:`here <shortcuts>`.

   .. code-block:: console

      (lӓut-host2) show ip route
      2024-07-26 17:28:00: %LAUT-INFO: +..............................................................................+
      2024-07-26 17:28:00: %LAUT-INFO: :                      Execute 'show ip route' on 'host2'                      :
      2024-07-26 17:28:00: %LAUT-INFO: +..............................................................................+
      
      2024-07-26 17:28:00,739: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip route' +++
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
      (lӓut-host2)

Include entries
---------------

An *include* entry is used to verify if something 'is' present in the output of an ``execute`` command.
and is required to be given in the form of a regex pattern.

For example, in the above ``execute show ip route``, to verify if '1.1.1.1' appears as
a directly connected route via Loopback0 one could use the following regex pattern:

   C\\s+1.1.1.1 is directly connected, Loopback0

To add an *include* entry, invoke ``execute`` with the optional argument '-i' which would prompt user for
the *include* entries once the output of the command is printed on the screen. As an example, the command
``execute show ip route -i`` with the aforementioned *include* entry would look like this:

.. code-block:: console

   (lӓut-host2) execute show ip route -i
   2024-07-26 15:45:11: %LAUT-INFO: +..............................................................................+
   2024-07-26 15:45:11: %LAUT-INFO: :                      Execute 'show ip route' on 'host2'                      :
   2024-07-26 15:45:11: %LAUT-INFO: +..............................................................................+
   
   2024-07-26 15:45:11,402: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip route' +++
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
   2024-07-26 15:45:11: %LAUT-INFO: +..............................................................................+
   2024-07-26 15:45:11: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-07-26 15:45:11: %LAUT-INFO: +..............................................................................+
   Enter pattern to INCLUDE (Press enter for multiple patterns): C\s+1.1.1.1 is directly connected, Loopback0
   (lӓut-host2)

LAUT autogenerates blitz *'execute'* action snippet when added with *include* entries and all added *include* entries
thereby appear in the autogenerated blitz snippet as a list under blitz *'include'* as seen below for the above example:

.. code-block:: console

   (lӓut-host2) list 1
   execute:
     device: host2
     command: show ip route
     include:
       - C\s+1.1.1.1 is directly connected, Loopback0
   (lӓut-host2)

Multiple *include* entries to verify more than one regex pattern can also be added in case users want to verify them.
To do so, enter the *'INCLUDE>'* prompt by simply press enter when prompted for the first *include* entry.
When under *'INCLUDE>'* prompt, keep adding multiple entries until you have reached the last pattern to input,
in which case just press <enter> once to exit this mode(i.e. a newline exits the *'INCLUDE>'* mode).

Here is an example where 3 routes (1.1.1.1, 2.2.2.2, 3.3.3.3) were verified in 'show ip route':

.. code-block:: console

   (lӓut-host2) show ip route -i
   2024-07-26 17:38:07: %LAUT-INFO: +..............................................................................+
   2024-07-26 17:38:07: %LAUT-INFO: :                      Execute 'show ip route' on 'host2'                      :
   2024-07-26 17:38:07: %LAUT-INFO: +..............................................................................+
   
   2024-07-26 17:38:07,900: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip route' +++
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
         2.0.0.0/32 is subnetted, 1 subnets
   C        2.2.2.2 is directly connected, Loopback1
         3.0.0.0/32 is subnetted, 1 subnets
   C        3.3.3.3 is directly connected, Loopback2
   host2#
   2024-07-26 17:38:08: %LAUT-INFO: +..............................................................................+
   2024-07-26 17:38:08: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-07-26 17:38:08: %LAUT-INFO: +..............................................................................+
   Enter pattern to INCLUDE (Press enter for multiple patterns):
   (INCLUDE)> C\s+1.1.1.1 is directly connected, Loopback0
   (INCLUDE)> C\s+2.2.2.2 is directly connected, Loopback1
   (INCLUDE)> C\s+3.3.3.3 is directly connected, Loopback2
   (INCLUDE)>
   (lӓut-host2)

The corresponding autogenerated code for the above ``execute`` command would look like this
consisting of multiple *'include'* entries:

.. code-block:: console

   (lӓut-host2) list 1
   execute:
     device: host2
     command: show ip route
     include:
       - C\s+1.1.1.1 is directly connected, Loopback0
       - C\s+2.2.2.2 is directly connected, Loopback1
       - C\s+3.3.3.3 is directly connected, Loopback2
   (lӓut-host2)

When user inputs a regex pattern that doesn't match the output, LAUT explicitly mentions it & prompts if
the user would still like to add this pattern as an *include* entry.
To better understand this, note the below example where we mistakenly add the pattern to route '3.3.3.3' as an
*include* entry without knowing that the route actually does not exist(in cases where maybe we may have forgot
that 3.3.3.3 is no longer in the test scenario). LAUT points out this by verifying the
regex pattern with the output of the command thereby preventing a mistake.

.. code-block:: console

   (lӓut-host2) show ip route -i
   2024-07-26 17:40:36: %LAUT-INFO: +..............................................................................+
   2024-07-26 17:40:36: %LAUT-INFO: :                      Execute 'show ip route' on 'host2'                      :
   2024-07-26 17:40:36: %LAUT-INFO: +..............................................................................+
   
   2024-07-26 17:40:36,258: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip route' +++
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
         2.0.0.0/32 is subnetted, 1 subnets
   C        2.2.2.2 is directly connected, Loopback1
   host2#
   2024-07-26 17:40:36: %LAUT-INFO: +..............................................................................+
   2024-07-26 17:40:36: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-07-26 17:40:36: %LAUT-INFO: +..............................................................................+
   Enter pattern to INCLUDE (Press enter for multiple patterns): C\s+3.3.3.3 is directly connected, Loopback2
   Adding this entry would cause this blitz action to fail
   Do you still want to add this pattern (y/n): n
   Enter pattern to INCLUDE (Press enter for multiple patterns):


Exclude entries
---------------

Adding an *exclude* entry is exactly the same as as adding an *include* entry, except that the added entry would check
if a particular regex pattern **DOES NOT** match the ``execute`` output.

To add an *exclude* entry, invoke ``execute`` with the optional argument '-e' which would prompt user for
the *exclude* entries once the output of the command is printed on the screen. To add an *exclude* entry along with
*include* entries, invoke the command ``execute <CMD> -i -e``.

Continuing with the previous example, if we *shut* the interface 'Loopback0'
it is expected that the route to '1.1.1.1' is expected to not show up in 'show ip route' command.
In this particular case, we should input the regex pattern that matches '1.1.1.1' route as an
*exclude* entry so as to assure that after the interface has been *shut* we shouldn't expect
the route to be present in the routing table:

.. code-block:: console

   (lӓut-host2) execute show ip route -e
   2024-07-26 17:48:51: %LAUT-INFO: +..............................................................................+
   2024-07-26 17:48:51: %LAUT-INFO: :                      Execute 'show ip route' on 'host2'                      :
   2024-07-26 17:48:51: %LAUT-INFO: +..............................................................................+
   
   2024-07-26 17:48:51,924: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip route' +++
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
   2024-07-26 17:48:52: %LAUT-INFO: +..............................................................................+
   2024-07-26 17:48:52: %LAUT-INFO: :                                   EXCLUDE                                    :
   2024-07-26 17:48:52: %LAUT-INFO: +..............................................................................+
   Enter pattern to EXCLUDE (Press enter for multiple patterns): C\s+1.1.1.1 is directly connected, Loopback0
   (lӓut-host2)

For the above *exclude* entry, following is the autogenerated blitz action snippet consisting
of the '1.1.1.1' route regex pattern as an *'exclude'* entry:

.. code-block:: console

   (lӓut-host2) list 1
   execute:
     device: host2
     command: show ip route
     exclude:
       - C\s+1.1.1.1 is directly connected, Loopback0
   (lӓut-host2)

One can also input multiple *exclude* entries in the same way as multiple *include* entries.
