Shell history
=============

LAUT stores the history of commands invoked permanently across multiple sessions
in the file '~/.laut_history.dat'. The advantage of this is that users can
perform <Up> & <Down> to navigate the history as well as perform reverse-i-search
akin to one in bash.

Advanced history
----------------

LAUT stores history as 5 parts with each being independent of the others:

1. Command history
2. *include*, *exclude*, *save* entry history
3. ``api`` argument history
4. LAUT-exec mode history
5. LAUT-cfg mode history

This was done to provide a separation between the different areas.

An example of reverse-i-search prompt appearing in *include* entry:

.. code-block:: console

   (lӓut-host1) show ip route -i
   2024-08-01 09:41:53: %LAUT-INFO: +..............................................................................+
   2024-08-01 09:41:53: %LAUT-INFO: :                      Execute 'show ip route' on 'host1'                      :
   2024-08-01 09:41:53: %LAUT-INFO: +..............................................................................+
   
   2024-08-01 09:41:54,083: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ip route' +++
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
   2024-08-01 09:41:54: %LAUT-INFO: +..............................................................................+
   2024-08-01 09:41:54: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-08-01 09:41:54: %LAUT-INFO: +..............................................................................+
   (reverse-i-search)`':

.. note::

   For mac users, please try to remap <CAPS_LOCK> key to <ctrl-r> using karabiner-elements.
   This way its much easier to always try reverse-i-search when typing something.
   To configure the remap, add a complex modification to karabiner-elements using the below JSON:

   .. code-block:: json

      {
       "description": "New Rule (change caps_lock to control-R)",
       "manipulators": [
           {
               "from": {
                   "key_code": "caps_lock"
               },
               "to": {
                   "key_code": "r",
                   "modifiers": [
                       "control"
                   ]
               },
               "type": "basic"
           }
       ]
      }
