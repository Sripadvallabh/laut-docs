LAUT Basics
===========

Learning Prerequisites
----------------------

In order to understand LAUT, it is required to understand pyATS & blitz first.
Please go through below links for related information.

    * `pyATS <https://developer.cisco.com/docs/pyats>`_
    * `pyATS Blitz <https://pubhub.devnetcloud.com/media/genie-docs/docs/blitz/index.html>`_

Load Testbed
-------------

LAUT works on network devices.
In order to obtain information about them, it needs a pyATS testbed YAML.

Following is a sample YAML that provides information about 2 IOS-XE devices:

.. code-block:: yaml

   devices:
     host1:
       os: iosxe
       type: iosxe
       platform: iol
       connections:
         a:
           ip: 127.0.0.1
           port: 33331
           protocol: telnet
         defaults:
           class: unicon.Unicon
           via: cli
     host2:
       os: iosxe
       type: iosxe
       platform: iol
       connections:
         a:
           ip: 127.0.0.1
           port: 33332
           protocol: telnet
         defaults:
           class: unicon.Unicon
           via: cli

To provide this information to LAUT, use the ``testbed`` command with
relative filepath to the testbed YAML as argument. Once a testbed YAML is provided
via ``testbed`` command, LAUT connects to each & every device in the testbed file
via pyATS ``connect()`` API & displays the '*Testbed load successful*' INFO message
when all devices are successfully connected.

.. code-block:: console

   (lӓut-TB-NONE) testbed pyats/testbed.yaml
   2024-07-26 14:04:30: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-26 14:04:30: %LAUT-INFO: :                     Loading testbed 'pyats/testbed.yaml'                     :
   2024-07-26 14:04:30: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-26 14:04:35: %LAUT-INFO: +..............................................................................+
   2024-07-26 14:04:35: %LAUT-INFO: :                  Connecting to all devices in given testbed                  :
   2024-07-26 14:04:35: %LAUT-INFO: +..............................................................................+
   2024-07-26 14:04:35: %LAUT-INFO: +..............................................................................+
   2024-07-26 14:04:35: %LAUT-INFO: :                         Connecting to device 'host1'                         :
   2024-07-26 14:04:35: %LAUT-INFO: +..............................................................................+
   
   2024-07-26 14:04:35,131: %UNICON-INFO: +++ host1 logfile /tmp/host1-cli-20240726T140435130.log +++
   
   2024-07-26 14:04:35,131: %UNICON-INFO: +++ Unicon plugin iosxe (unicon.internal.plugins.iosxe) +++
   /nobackup/araradha/pyats/lib/python3.8/site-packages/unicon/bases/routers/connection.py:97: DeprecationWarning: Arguments 'username', 'enable_password','tacacs_password' and 'line_password' are now deprecated and replaced by 'credentials'.
     warnings.warn(message = "Arguments 'username', "
   Trying 127.0.0.1...
   Connected to 127.0.0.1.
   Escape character is '^]'.
   
   
   2024-07-26 14:04:35,176: %UNICON-INFO: +++ connection to spawn: telnet 127.0.0.1 33331, id: 140026962501840 +++
   
   2024-07-26 14:04:35,176: %UNICON-INFO: connection to host1
   
   host1#
   
   host1#
   
   2024-07-26 14:04:36,360: %UNICON-INFO: +++ host1 with via 'a': executing command 'show version | include operating mode' +++
   show version | include operating mode
   host1#
   
   2024-07-26 14:04:36,540: %UNICON-INFO: +++ initializing handle +++
   
   2024-07-26 14:04:36,692: %UNICON-INFO: +++ host1 with via 'a': executing command 'term length 0' +++
   term length 0
   host1#
   2024-07-26 14:04:36: %LAUT-INFO: +..............................................................................+
   2024-07-26 14:04:36: %LAUT-INFO: :                         Connecting to device 'host2'                         :
   2024-07-26 14:04:36: %LAUT-INFO: +..............................................................................+
   
   2024-07-26 14:04:36,999: %UNICON-INFO: +++ host2 logfile /tmp/host2-cli-20240726T140436997.log +++
   
   2024-07-26 14:04:36,999: %UNICON-INFO: +++ Unicon plugin iosxe (unicon.internal.plugins.iosxe) +++
   Trying 127.0.0.1...
   Connected to 127.0.0.1.
   Escape character is '^]'.
   
   
   2024-07-26 14:04:37,058: %UNICON-INFO: +++ connection to spawn: telnet 127.0.0.1 33332, id: 140026959604368 +++
   
   2024-07-26 14:04:37,058: %UNICON-INFO: connection to host2
   
   host2#
   
   host2#
   
   2024-07-26 14:04:38,122: %UNICON-INFO: +++ host2 with via 'a': executing command 'show version | include operating mode' +++
   show version | include operating mode
   host2#
   
   2024-07-26 14:04:38,269: %UNICON-INFO: +++ initializing handle +++
   
   2024-07-26 14:04:38,445: %UNICON-INFO: +++ host2 with via 'a': executing command 'term length 0' +++
   term length 0
   host2#
   2024-07-26 14:04:38: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-26 14:04:38: %LAUT-INFO: :                           Testbed load successful                            :
   2024-07-26 14:04:38: %LAUT-INFO: +------------------------------------------------------------------------------+
   (lӓut-host1)


Switch between devices
----------------------

Once a testbed loads successfully in LAUT shell, switch between multiple devices
in the testbed via the ``device`` command. LAUT shell prompt always reflects the device currently *active*
in the shell in the format ``(lӓut-<ACTIVE_DEVICE_HOSTNAME>)``.

For example, switch from device 'host1' to 'host2' by running the
``device`` command with 'host2' as argument:

.. code-block:: console

    (lӓut-host1) device host2
    (lӓut-host2)

Note the LAUT prompt changed from 'host1' to 'host2' after running the ``device`` command
indicating that the current active device in the shell is 'host2'.

Basic device operations
-----------------------

There are 2 basic operations that can be performed on any device:

    * Configure  : Used to configure a particular CLI
    * Execute    : Used to run a command in device exec prompt

**Configure**

To configure any CLI on a device, switch to that device using ``device`` command
& then run the ``configure`` command with the CLI as an argument.

In the example below, 'no logging console' was configured on the device 'host2':

.. code-block:: console

   (lӓut-host2) configure no logging console
   2024-07-07 13:39:05: %LAUT-INFO: +..............................................................................+
   2024-07-07 13:39:05: %LAUT-INFO: :                  Configure 'no logging console' on 'host2'                   :
   2024-07-07 13:39:05: %LAUT-INFO: +..............................................................................+

   2024-07-07 13:39:05,563: %UNICON-INFO: +++ host2 with via 'cli': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   host2(config)#no logging console
   host2(config)#end
   host2#
   (lӓut-host2)

Config application on the device will always be printed on the terminal screen.

To configure multiple CLI, type ``configure`` and hit Enter. This enters *'LAUT-cfg'* mode
which mimics config terminal prompt in device. After configuring the required CLI, type 'end' to exit &
return back to LAUT shell prompt.

See example below for a loopback interface configuration done via *'LAUT-cfg'* mode:

.. code-block:: console

   (lӓut-host2) configure
   (host2:config)> interface Loopback0
   (host2:config-if)> ip address 1.1.1.1 255.255.255.255
   (host2:config-if)> end
   (lӓut-host2)

**Execute**

To execute any command on a device, switch to that device using ``device`` command
& then run the ``execute`` command.

There is a shortcut to execute 'show' commands by just invoking ``show`` instead 
of ``execute show``.

In the example below, 'show ip route' was executed on the device 'host2':

.. code-block:: console

   (lӓut-host2) show ip route
   2024-07-26 14:26:20: %LAUT-INFO: +..............................................................................+
   2024-07-26 14:26:20: %LAUT-INFO: :                      Execute 'show ip route' on 'host1'                      :
   2024-07-26 14:26:20: %LAUT-INFO: +..............................................................................+
   
   2024-07-26 14:26:20,197: %UNICON-INFO: +++ host1 with via 'a': executing command 'show ip route' +++
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

The output from the command execution will be printed on the terminal screen.

Validate execute outputs
------------------------

Blitz has the concept of *'include'* & *'exclude'* entries to verify ``execute`` outputs.

    * An *'include'* entry can verify if a particular pattern appears in the output
    * An *'exclude'* entry can verify if a particular pattern **DOES NOT** appear in the output

To add *include* entries to an ``execute`` command output, invoke the ``execute`` command
with an argument '-i' as in ``execute -i`` which in turn does normal command execution
and asks for the user to input 'include' entries.

For example, to verify if '1.1.1.1' is reachable via Loopback0 in the output of
'sh ip route', an 'include' entry 'C\\s+1.1.1.1 is directly connected, Loopback0' can
be provided as shown below:

.. code-block:: console

   (lӓut-host2) show ip route -i
   2024-07-26 15:17:46: %LAUT-INFO: +..............................................................................+
   2024-07-26 15:17:46: %LAUT-INFO: :                      Execute 'show ip route' on 'host2'                      :
   2024-07-26 15:17:46: %LAUT-INFO: +..............................................................................+
   
   2024-07-26 15:17:46,257: %UNICON-INFO: +++ host2 with via 'a': executing command 'show ip route' +++
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
   2024-07-26 15:17:46: %LAUT-INFO: +..............................................................................+
   2024-07-26 15:17:46: %LAUT-INFO: :                                   INCLUDE                                    :
   2024-07-26 15:17:46: %LAUT-INFO: +..............................................................................+
   Enter pattern to INCLUDE (Press enter for multiple patterns): C\s+1.1.1.1 is directly connected, Loopback0

Similarily, to add *exclude* entries to an ``execute`` command output, invoke the ``execute`` command
with the argument '-e'.

The presence of *include* & 'exclude' entries connected with an ``execute`` output
will provide the autogenerated LAUT pyATS blitz script with the means to verify certain values in an ``execute`` output thus
forming the basis of any verification.

View generated Blitz snippets
-----------------------------

For every ``execute -i/-e`` and ``configure`` command, a corresponding blitz snippet
code consisting of blitz *'execute'* and *'configure'* action respectively will be
autogenerated by LAUT. To view the autogenerated snippet, LAUT provides the ``list``
command. ``list`` command displays the last 'n' autogenerated blitz action snippets.

For the command ``execute sh ip route -i`` tried earlier, the corresponding autogenerated
blitz action snippet could be thereby observed via the command ``list 1``.

.. code-block:: console

   (lӓut-host2) list 1
   execute:
     device: host2
     command: show ip route
     include:
       - C\s+1.1.1.1 is directly connected, Loopback0

As seen above, LAUT autogenerated the previous *'execute'* blitz action snippet along
with the *include* entry to verify the route for '1.1.1.1'.

Below is another example showing the autogenerated *'configure'* action snippet
for the ``configure no logging console`` command tried earlier.

.. code-block:: console

   (lӓut-host2) list 1
   configure:
     device: host2
     command: no logging console

Save snippets as Blitz testcase
-------------------------------

Configuration of multiple CLI along with execute verifications across multiple
devices brings together a series of autogenerated blitz snippets which when put together
form a basic blitz testcase.

So far, we have tried to create a loopback interface with an IP address and checked
whether a connected route was populated in the routing table using 'sh ip route' command.
To see all the autogenerated blitz snippets, use the LAUT command ``list -a``.

.. code-block:: console

   (lӓut-host2) list -a
   default:
     - configure:
         device: host2
         command: no logging console
     - configure:
         device:
           - host2
         command: |-
           interface Loopback0
           ip address 1.1.1.1 255.255.255.255
     - execute:
         device: host2
         command: show ip route
         include:
           - C\s+1.1.1.1 is directly connected, Loopback0

Then to form all of these blitz action snippets together into a testcase, use the LAUT
``save`` command with an argument like 'pyats/testcases/TC1.yaml' (which would essentially mean that the
testcase would be named 'TC1' and its blitz YAML would be saved in 'pyats/testcases/' directory).

.. code-block:: console

   (lӓut-host2) save pyats/testcases/TC1.yaml
   2024-07-26 15:30:54: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-26 15:30:54: %LAUT-INFO: :              File 'pyats/testcases/TC1.yaml' saved successfully              :
   2024-07-26 15:30:54: %LAUT-INFO: +------------------------------------------------------------------------------+
   (lӓut-host2)

After exiting LAUT shell using the LAUT ``exit`` command (which would disconnect from all connected
devices mentioned in the testbed), 'TC1.yaml' would probably look like this:

.. code-block:: console

   (lӓut-host2) exit
   2024-07-26 15:31:21: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-26 15:31:21: %LAUT-INFO: :                  Disconnecting from all devices in testbed                   :
   2024-07-26 15:31:21: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-26 15:31:21: %LAUT-INFO: :                      Disconnecting from device 'host1'                       :
   2024-07-26 15:31:21: %LAUT-INFO: :                      Disconnecting from device 'host2'                       :
   2024-07-26 15:31:21: %LAUT-INFO: +------------------------------------------------------------------------------+
   
   🎃 Thank you for using LAUT

   $ vim pyats/testcases/TC1.yaml

.. code-block:: yaml

   # TC1.yaml
   # 26 July 2024
   # LAUT Generated testcase
   TC1:
     source:
       pkg: genie.libs.sdk
       class: triggers.blitz.blitz.Blitz
     devices:
       - host2
     test_sections:
       - default:
           - configure:
               device: host2
               command: no logging console
           - configure:
               device:
                 - host2
               command: |-
                 interface Loopback0
                 ip address 1.1.1.1 255.255.255.255
           - execute:
               device: host2
               command: show ip route
               include:
                 - C\s+1.1.1.1 is directly connected, Loopback0


Adding 'TC1.yaml' to a pyATS *main_trigger_datafile* will make the above
testcase part of an pyATS AUT script.

What's next
------------

With basics geared, a closer at look at each of the individual LAUT commands
helps to better understand their nuances and intricacies. Finally, one may take a look
at the multitude of LAUT features one by one to see if they can enhance your workflow
and fit in any of your requirements. Happy learning!
