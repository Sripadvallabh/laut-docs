Builtins
=========

These commands work at the shell & OS level to enhance LAUT's working experience.
Mastering them can considerably improve workflow efficiency. All of these commands
are provided by the base ``cmd2`` module. Read
`cmd2 builtin commands <https://cmd2.readthedocs.io/en/latest/features/builtin_commands.html>`_
to learn more.

alias
-----
Manage aliases for commands.

Create aliases using ``alias create``; ``alias create x execute`` maps 'x'
to the command 'execute':

.. code-block:: console

   (lӓut-l2switch) alias create x execute
   Alias 'x' created
   (lӓut-l2switch) x show ip route
   2024-07-12 17:34:20: %LAUT-INFO: +..............................................................................+
   2024-07-12 17:34:20: %LAUT-INFO: :                    Execute 'show ip route' on 'l2switch'                     :
   2024-07-12 17:34:20: %LAUT-INFO: +..............................................................................+

   2024-07-12 17:34:20,824: %UNICON-INFO: +++ l2switch with via 'cli': executing command 'show ip route' +++
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
   Add INCLUDE section (y/n): n
   Add EXCLUDE section (y/n): n
   Add SAVE section (y/n): n
   (lӓut-l2switch)

See created aliases using ``alias list``:

.. code-block:: console

   (lӓut-l2switch) alias list
   alias create x execute
   (lӓut-l2switch)

Delete created aliases using ``alias delete``:

.. code-block:: console

   (lӓut-l2switch) alias delete x
   Alias 'x' deleted
   (lӓut-l2switch)

edit
-----
Run a text editor and optionally open a file with it.

To edit 'pyats/parameters.yaml' from LAUT shell, just give the following command
as shown below which opens the file in your text editor:

.. code-block:: console

   (lӓut-l2switch) edit pyats/parameters.yaml

history
-------
View, run, edit, save, or clear previously entered commands.

.. code-block:: console

   (lӓut-l2switch) history
    1001  testbed pyats/testbed.yaml
    1002  device l2switch
    1003  alias create x execute
    1004  x show ip route
    1005  alias list
    1006  alias delete x
    1007  !cat .lautrc
    1008  edit pyats/parameters.yaml
   (lӓut-l2switch)

More information `here <https://cmd2.readthedocs.io/en/latest/features/history.html#for-users>`_.

macro
------
Manage macros for commands.

Its similar to the ``alias`` command but can contain argument placeholders.
Arguments are expressed when creating a macro using {#} notation where {1} means the first argument.

An example where a macro named 'r' was created for the command ``execute show ip route`` with an
argument placeholder for the IP address; ``r 5.5.5.5`` would show the route for '5.5.5.5', ``r 6.6.6.6``
would show the route for '6.6.6.6':

.. code-block:: console

   (lӓut-l2switch) macro create r execute show ip route {1}
   Macro 'r' created
   (lӓut-l2switch) r 5.5.5.5
   2024-07-12 18:50:28: %LAUT-INFO: +..............................................................................+
   2024-07-12 18:50:28: %LAUT-INFO: :                Execute 'show ip route 5.5.5.5' on 'l2switch'                 :
   2024-07-12 18:50:28: %LAUT-INFO: +..............................................................................+

   2024-07-12 18:50:28,861: %UNICON-INFO: +++ l2switch with via 'cli': executing command 'show ip route 5.5.5.5' +++
   show ip route 5.5.5.5
   Routing entry for 5.5.5.5/32
     Known via "connected", distance 0, metric 0 (connected, via interface)
     Routing Descriptor Blocks:
     * directly connected, via Loopback0
         Route metric is 0, traffic share count is 1
   l2switch#
   Add INCLUDE section (y/n): n
   Add EXCLUDE section (y/n): n
   Add SAVE section (y/n): n
   (lӓut-l2switch)
   (lӓut-l2switch) r 6.6.6.6
   2024-07-12 18:51:53: %LAUT-INFO: +..............................................................................+
   2024-07-12 18:51:53: %LAUT-INFO: :                Execute 'show ip route 6.6.6.6' on 'l2switch'                 :
   2024-07-12 18:51:53: %LAUT-INFO: +..............................................................................+

   2024-07-12 18:51:53,394: %UNICON-INFO: +++ l2switch with via 'cli': executing command 'show ip route 6.6.6.6' +++
   show ip route 6.6.6.6
   Routing entry for 6.6.6.6/32
     Known via "connected", distance 0, metric 0 (connected, via interface)
     Routing Descriptor Blocks:
     * directly connected, via Loopback1
         Route metric is 0, traffic share count is 1
   l2switch#
   Add INCLUDE section (y/n): n
   Add EXCLUDE section (y/n): n
   Add SAVE section (y/n): n
   (lӓut-l2switch)

To view all macros, use ``macro list``:

.. code-block:: console

   (lӓut-l2switch) macro list
   macro create r execute show ip route {1}
   (lӓut-l2switch)

To delete a particular macro, use ``macro delete``:

.. code-block:: console

   (lӓut-l2switch) macro delete r
   Macro 'r' deleted
   (lӓut-l2switch)

py
---
Launch an interactive Python shell.

.. code-block:: console

   (lӓut-leaf2-lag2) py
   Python 3.8.2 (default, May 14 2020, 01:47:43) 
   [GCC 8.3.1 20190507 (Red Hat 8.3.1-4)] on linux
   Type "help", "copyright", "credits" or "license" for more information.

   Use `Ctrl-D` (Unix) / `Ctrl-Z` (Windows), `quit()`, `exit()` to exit.
   Run CLI commands with: app("command ...")

   >>> 1 + 2 + 3
   6
   >>> exit()
   Now exiting Python shell...
   (lӓut-leaf2-lag2)

run_script
-----------
Run a script file consisting of LAUT shell commands.

run_pyscript
------------
Run a python script file inside LAUT shell.

set
----
Modify & view LAUT settings.

shell
------
Execute a command as if at the OS prompt.

.. _shortcuts:

shortcuts
----------
List available shortcuts.
