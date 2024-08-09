Builtin commands
================

These commands work at the shell & OS level to enhance user working experience.

alias
-----
Having alises for commands prevents unnecessary typing for frequently used commands.

Create aliases using ``alias create``; ``alias create x execute`` maps 'x'
to the command 'execute':

.. code-block:: console

   (lӓut-host1) alias create c configure
   Alias 'c' created
   (lӓut-host1) c no logging console
   2024-07-31 15:01:16: %LAUT-INFO: +..............................................................................+
   2024-07-31 15:01:16: %LAUT-INFO: :                  Configure 'no logging console' on 'host1'                   :
   2024-07-31 15:01:16: %LAUT-INFO: +..............................................................................+
   
   2024-07-31 15:01:16,694: %UNICON-INFO: +++ host1 with via 'a': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   host1(config)#no logging console
   host1(config)#end
   host1#
   (lӓut-host1)

See created aliases using ``alias list``:

.. code-block:: console

   (lӓut-host1) alias list
   alias create c configure
   (lӓut-host1)

Delete created aliases using ``alias delete``:

.. code-block:: console

   (lӓut-host1) alias delete c
   Alias 'c' deleted
   (lӓut-host1) c no logging console
   c is not a recognized command, alias, or macro
   (lӓut-host1)

It is useful to create aliases for frequent specific commands; for example, if an user wants to frequently
check for the 'show' command ``show ip interface brief``, it is better to create an alias named 'int' via
``alias create int show ip interface brief``.

macro
------

Its similar to the ``alias`` command but can contain argument placeholders.
Arguments are expressed when creating a macro using {#} notation where {1} means the first argument.

An example where a macro named 'route' was created for the command ``show ip route`` with an
argument placeholder for the IP address; ``route 5.5.5.5`` would show the route for '5.5.5.5', ``route 6.6.6.6``
would show the route for '6.6.6.6':

.. code-block:: console

   (lӓut-host1) macro create route show ip route {1}
   Macro 'route' created
   (lӓut-host1) 
   (lӓut-host1) route 1.1.1.1
   2024-07-31 15:06:23: %LAUT-INFO: +..............................................................................+
   2024-07-31 15:06:23: %LAUT-INFO: :                  Execute 'show ip route 1.1.1.1' on 'host1'                  :
   2024-07-31 15:06:23: %LAUT-INFO: +..............................................................................+
   
   2024-07-31 15:06:23,244: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ip route 1.1.1.1' +++
   show ip route 1.1.1.1
   Routing entry for 1.1.1.1/32
     Known via "connected", distance 0, metric 0 (connected, via interface)
     Routing Descriptor Blocks:
     * directly connected, via Loopback0
         Route metric is 0, traffic share count is 1
   host1#
   (lӓut-host1)
   (lӓut-host1) route 5.5.5.5
   2024-07-31 15:05:52: %LAUT-INFO: +..............................................................................+
   2024-07-31 15:05:52: %LAUT-INFO: :                  Execute 'show ip route 5.5.5.5' on 'host1'                  :
   2024-07-31 15:05:52: %LAUT-INFO: +..............................................................................+
   
   2024-07-31 15:05:52,308: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ip route 5.5.5.5' +++
   show ip route 5.5.5.5
   % Network not in table
   host1#

To view all macros, use ``macro list``:

.. code-block:: console

   (lӓut-host1) macro list
   macro create route show ip route {1}
   (lӓut-host1)

To delete a particular macro, use ``macro delete``:

.. code-block:: console

   (lӓut-host1) macro delete route
   Macro 'route' deleted
   (lӓut-host1) route 1.1.1.1
   route is not a recognized command, alias, or macro
   (lӓut-host1)

.. _shortcuts:

shortcuts
----------
Shortcuts are default 'aliases' created & inbuilt in the shell. ``shortcuts`` command lists them.

.. code-block:: console

   (lӓut-host1) shortcuts
   Shortcuts for other commands:
   !: shell
   #: run_pyscript
   %: parameter list
   ?: help
   @: run_script
   @@: _relative_run_script
   ^: variable list
   show: execute show
   (lӓut-host1)

The most important of these are shown in the table below:

.. list-table:: Important shortcuts
   :widths: 50 50
   :header-rows: 1

   * - Shortcut
     - Actual command
   * - show
     - execute show
   * - !
     - shell
   * - %
     - parameter list
   * - ^
     - variable list

The reason why 'show' commands work directly without the command name 'execute' in front of them
is because of the shortcut 'show' mapping to ``execute show``:

.. code-block:: console

   (lӓut-host1) show ip route
   2024-07-31 15:34:32: %LAUT-INFO: +..............................................................................+
   2024-07-31 15:34:32: %LAUT-INFO: :                      Execute 'show ip route' on 'host1'                      :
   2024-07-31 15:34:32: %LAUT-INFO: +..............................................................................+
   
   2024-07-31 15:34:32,564: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ip route' +++
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
   (lӓut-host1)

edit
-----
Run a text editor and optionally open a file with it.

As an example, to edit 'pyats/parameters.yaml' from LAUT shell, just give the following command
as shown below which opens the file in your text editor:

.. code-block:: console

   (lӓut-host1) edit pyats/parameters.yaml

Both relative and absolute paths can be given as arguments; the relative path works from the directory
where LAUT was started.

shell
------
Execute a command as if at the OS prompt.

For example, to view files using 'ls', ``shell ls`` could be used:

.. code-block:: console

   (lӓut-host1) shell ls ../../alternate_msdp/
   destroy_iol					   mvpn-inter-as-ut-topo  nvram_00014	vimsession.vim
   extra						   NETMAP		  pnp-info	x86_64bi_linux-adventerprise-ms
   i86bi_linux-tpgen_adventerprisek9-ms.PAGENT.5.0.0  nvram_00011		  pnp-tech	xwrapper
   iol-linux-manual-startup			   nvram_00012		  testbed.yaml
   laut.bin					   nvram_00013		  undo.undo
   (lӓut-host1)

set 
---
Used to modify settings provided by LAUT shell.

List of all settings:

.. code-block:: console

   (lӓut-leaf2) set
   Name                    Value                           Description
   ====================================================================================================================
   blitz_continue_enabled  False                           Enable blitz's continue feature
   blitz_exp_fail_enabled  False                           Enable blitz's expected_failure feature
   blitz_group_enabled     False                           Enable blitz's group feature
   blitz_retry             0/0                             Blitz retry mechanism; format: max_time/check_interval
   console_logs_dir        None                            Console logs directory path
   (lӓut-leaf2)

Modify a setting via the following way ``set <SETTING_NAME> <VALUE>``:

.. code-block:: console

   (lӓut-leaf2) set blitz_continue_enabled true
   blitz_continue_enabled - was: False
   now: True
   (lӓut-leaf2)
