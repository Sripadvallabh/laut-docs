
.. _basics:

LAUT Basics
===========

Learning Prerequisites
----------------------

In order to understand LAUT, it is better to understand pyATS & Blitz first.
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
      leaf2-lag2:
        os: iosxe
        type: iosxe
        connections:
          cli:
            class: unicon.Unicon
            ip: 10.78.238.163
            port: 22
            protocol: ssh
          defaults:
            class: unicon.Unicon
            via: cli
        credentials:
          default:
            username: admin
            password: lab
      l2switch:
        os: iosxe
        type: iosxe
        connections:
          cli:
            class: unicon.Unicon
            ip: 10.78.238.164
            port: 22
            protocol: ssh
          defaults:
            class: unicon.Unicon
            via: cli
        credentials:
          default:
            username: admin
            password: lab

To provide this information to LAUT, use the ``testbed`` command with
relative filepath to the testbed YAML as argument. Once a testbed YAML is provided
via ``testbed`` command, LAUT connects to each & every device in the testbed file
via pyATS ``connect()`` API & displays the '*Testbed load successful*' INFO message
when all devices are connected.

.. code-block:: console

    (lӓut-TB-NONE) testbed pyats/testbed.yaml
    2024-07-06 16:54:29: %LAUT-INFO: +------------------------------------------------------------------------------+
    2024-07-06 16:54:29: %LAUT-INFO: :                     Loading testbed 'pyats/testbed.yaml'                     :
    2024-07-06 16:54:29: %LAUT-INFO: +------------------------------------------------------------------------------+
    2024-07-06 16:54:29: %LAUT-INFO: +..............................................................................+
    2024-07-06 16:54:29: %LAUT-INFO: :                  Connecting to all devices in given testbed                  :
    2024-07-06 16:54:29: %LAUT-INFO: +..............................................................................+
    2024-07-06 16:54:29: %LAUT-INFO: +..............................................................................+
    2024-07-06 16:54:29: %LAUT-INFO: :                      Connecting to device 'leaf2-lag2'                       :
    2024-07-06 16:54:29: %LAUT-INFO: +..............................................................................+

    2024-07-06 16:54:29,189: %UNICON-INFO: +++ leaf2-lag2 logfile /tmp/leaf2-lag2-cli-20240706T165429189.log +++

    2024-07-06 16:54:29,190: %UNICON-INFO: +++ Unicon plugin iosxe (unicon.internal.plugins.iosxe) +++
    Warning: Permanently added '10.78.238.163' (RSA) to the list of known hosts.


    2024-07-06 16:54:29,523: %UNICON-INFO: +++ connection to spawn: ssh -l admin 10.78.238.163 -p 22 -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null, id: 139867410961648 +++

    2024-07-06 16:54:29,524: %UNICON-INFO: connection to leaf2-lag2
    Password:



    leaf2-lag2#

    leaf2-lag2#

    2024-07-06 16:54:34,135: %UNICON-INFO: +++ leaf2-lag2 with via 'cli': executing command 'show version | include operating mode' +++
    show version | include operating mode
    leaf2-lag2#

    2024-07-06 16:54:34,336: %UNICON-INFO: +++ initializing handle +++

    2024-07-06 16:54:34,459: %UNICON-INFO: +++ leaf2-lag2 with via 'cli': executing command 'term length 0' +++
    term length 0
    leaf2-lag2#
    2024-07-06 16:54:34: %LAUT-INFO: +..............................................................................+
    2024-07-06 16:54:34: %LAUT-INFO: :                       Connecting to device 'l2switch'                        :
    2024-07-06 16:54:34: %LAUT-INFO: +..............................................................................+

    2024-07-06 16:54:34,744: %UNICON-INFO: +++ l2switch logfile /tmp/l2switch-cli-20240706T165434743.log +++

    2024-07-06 16:54:34,744: %UNICON-INFO: +++ Unicon plugin iosxe (unicon.internal.plugins.iosxe) +++
    Warning: Permanently added '10.78.238.164' (RSA) to the list of known hosts.


    2024-07-06 16:54:35,083: %UNICON-INFO: +++ connection to spawn: ssh -l admin 10.78.238.164 -p 22 -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null, id: 139867411030752 +++

    2024-07-06 16:54:35,083: %UNICON-INFO: connection to l2switch
    Password:



    l2switch#

    l2switch#

    2024-07-06 16:54:39,631: %UNICON-INFO: +++ l2switch with via 'cli': executing command 'show version | include operating mode' +++
    show version | include operating mode
    l2switch#

    2024-07-06 16:54:39,821: %UNICON-INFO: +++ initializing handle +++

    2024-07-06 16:54:39,944: %UNICON-INFO: +++ l2switch with via 'cli': executing command 'term length 0' +++
    term length 0
    l2switch#
    2024-07-06 16:54:40: %LAUT-INFO: +------------------------------------------------------------------------------+
    2024-07-06 16:54:40: %LAUT-INFO: :                           Testbed load successful                            :
    2024-07-06 16:54:40: %LAUT-INFO: +------------------------------------------------------------------------------+
    (lӓut-leaf2-lag2)

Switch between devices
----------------------

Once a testbed loads successfully in LAUT shell, switch between multiple devices
in the testbed via the ``device`` command. LAUT shell prompt always reflects the device currently *active*
in the shell in the format ``(lӓut-<ACTIVE_DEVICE_HOSTNAME>)``.

For example, switch from device 'leaf2-lag2' to 'l2switch' by running the
``device`` command with 'l2switch' as argument:

.. code-block:: console

    (lӓut-leaf2-lag2) device l2switch
    (lӓut-l2switch)

Note the prompt changed from 'leaf2-lag2' to 'l2switch' after running the ``device`` command.

Basic device operations
-----------------------

There are 2 basic operations that can be performed on any device:

    * Execute    : Used to run a command in device exec prompt
    * Configure  : Used to configure a particular CLI

**Execute**

To execute any command on a device, switch to that device using ``device`` command
& then run the ``execute`` command. In the example below, 'sh ip route' was executed
on the device 'l2switch':

.. code-block:: console

   (lӓut-leaf2-lag2) device l2switch
   (lӓut-l2switch) execute show ip route
   2024-07-07 13:33:05: %LAUT-INFO: +..............................................................................+
   2024-07-07 13:33:05: %LAUT-INFO: :                    Execute 'show ip route' on 'l2switch'                     :
   2024-07-07 13:33:05: %LAUT-INFO: +..............................................................................+

   2024-07-07 13:33:05,912: %UNICON-INFO: +++ l2switch with via 'cli': executing command 'show ip route' +++
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
         6.0.0.0/32 is subnetted, 1 subnets
   C        6.6.6.6 is directly connected, Loopback1
         7.0.0.0/32 is subnetted, 1 subnets
   C        7.7.7.7 is directly connected, Loopback2
   l2switch#
   Add INCLUDE section (y/n): n
   Add EXCLUDE section (y/n): n
   Add SAVE section (y/n): n
   (lӓut-l2switch)

The output of the command execution will be printed on the terminal screen along
with a prompt requesting the user to input INCLUDE,EXCLUDE,SAVE sections(These 3
sections are specific to blitz and will be discussed next).

**Configure**

To configure any CLI on a device, switch to that device using ``device`` command
& then run the ``configure`` command. In the example below, 'logging console' was
configured on the device 'l2switch':

.. code-block:: console

   (lӓut-l2switch) configure logging console
   2024-07-07 13:39:05: %LAUT-INFO: +..............................................................................+
   2024-07-07 13:39:05: %LAUT-INFO: :                  Configure 'logging console' on 'l2switch'                   :
   2024-07-07 13:39:05: %LAUT-INFO: +..............................................................................+

   2024-07-07 13:39:05,563: %UNICON-INFO: +++ l2switch with via 'cli': configure +++
   config term
   Enter configuration commands, one per line.  End with CNTL/Z.
   l2switch(config)#logging console
   l2switch(config)#end
   l2switch#
   (lӓut-l2switch)

The output of config application will always be printed on the terminal screen.

View generated Blitz snippets
-----------------------------

For every ``execute`` and ``configure`` command, a corresponding blitz snippet
code consisting of blitz *'execute'* and *'configure'* action respectively will be
autogenerated by LAUT. To view the autogenerated snippet, LAUT provides the ``list``
command. ``list`` command displays the last 'n' autogenerated blitz action snippets in
reverse order.

In the below example, after giving ``execute sh ip route``, the corresponding autogenerated
blitz action snippet could be observed via the command ``list 1``.

.. code-block:: console

   (lӓut-l2switch) list 1
               - execute:
                   device: l2switch
                   command: ['show ip route']
   (lӓut-l2switch)

Below is another example showing the autogenerated *'configure'* action snippet
for the ``configure logging console`` command tried earlier.

.. code-block:: console

   (lӓut-l2switch) list 1
            - configure:
                device: l2switch
                command: ['logging console']
   (lӓut-l2switch)

Blitz specific inputs
---------------------

Blitz has the concept of *include* & *exclude* to verify outputs.

    * *include*: Verify if a particular pattern appears in the output
    * *exclude*: Verify if a particular pattern DOES NOT appear in the output

As seen previously, ``execute`` command prompted the user to input the INCLUDE &
EXCLUDE sections. Any user input to the INCLUDE section for ``execute`` command
will regex match with the output to check if it's present. For the EXCLUDE section,
regex match will check for the pattern to not be present in output.

For example, to verify if '5.5.5.5' is reachable via Loopback0 in the output of
'sh ip route', an input of 'C        5.5.5.5 is directly connected, Loopback0' is given
to INCLUDE section as shown below:

.. code-block:: console

   (lӓut-l2switch) execute show ip route
   2024-07-07 13:49:46: %LAUT-INFO: +..............................................................................+
   2024-07-07 13:49:46: %LAUT-INFO: :                    Execute 'show ip route' on 'l2switch'                     :
   2024-07-07 13:49:46: %LAUT-INFO: +..............................................................................+

   2024-07-07 13:49:46,946: %UNICON-INFO: +++ l2switch with via 'cli': executing command 'show ip route' +++
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
         6.0.0.0/32 is subnetted, 1 subnets
   C        6.6.6.6 is directly connected, Loopback1
         7.0.0.0/32 is subnetted, 1 subnets
   C        7.7.7.7 is directly connected, Loopback2
   l2switch#
   Add INCLUDE section (y/n): y
   Enter pattern to INCLUDE (Press enter for multiple patterns): C        5.5.5.5 is directly connected, Loopback0
   Add EXCLUDE section (y/n): n
   Add SAVE section (y/n): n

The presence of INCLUDE & EXCLUDE sections connected with an ``execute`` output
will provide blitz with the means to verify certain values in an ``execute`` output thus
forming the basis of verifications in an pyATS Blitz AUT script.

LAUT will autogenerate the corresponding *'execute'* blitz action snippet along
with the INCLUDE section as shown below:

.. code-block:: console

   (lӓut-l2switch) list 1
               - execute:
                device: l2switch
                command: ['show ip route']
                include:
                    - 'C        5.5.5.5 is directly connected, Loopback0'
   (lӓut-l2switch)

Save snippets as Blitz testcase
-------------------------------

Configuration of multiple CLI along with execute verifications across multiple
devices brings together a series of autogenerated blitz snippets when put together
forms a Blitz testcase. To make a Blitz testcase out of all the autogenerated blitz snippets
so far, use ``save`` command. It takes a filepath as argument.

.. code-block:: console

   (lӓut-l2switch) list 3
               - execute:
                   device: l2switch
                   command: ['show ip route']
                   include:
                       - 'C        5.5.5.5 is directly connected, Loopback0'
               - configure:
                   device: l2switch
                   command: ['logging console']
               - execute:
                   device: l2switch
                   command: ['show ip route']
   (lӓut-l2switch)
   (lӓut-l2switch) save pyats/testcases/TC1.yaml
   LAUT-INFO: File 'pyats/testcases/TC1.yaml' saved successfully
   (lӓut-l2switch)

After exiting using ``exit`` command which disconnects from all connected
devices mentioned in the testbed, 'TC1.yaml' would look like this:

.. code-block:: console

   (lӓut-l2switch) exit
   2024-07-07 14:24:00: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-07 14:24:00: %LAUT-INFO: :                  Disconnecting from all devices in testbed                   :
   2024-07-07 14:24:00: %LAUT-INFO: +------------------------------------------------------------------------------+
   2024-07-07 14:24:00: %LAUT-INFO: :                    Disconnecting from device 'leaf2-lag2'                    :
   2024-07-07 14:24:00: %LAUT-INFO: :                     Disconnecting from device 'l2switch'                     :
   2024-07-07 14:24:00: %LAUT-INFO: +------------------------------------------------------------------------------+

   🎃 Thank you for using LAUT

   $ vim pyats/testcases/TC1.yaml

.. code-block:: yaml

   # TC1.yaml
   # LAUT Generated testcase

   TC1:
       source:
           pkg: genie.libs.sdk
           class: triggers.blitz.blitz.Blitz
       devices: ['l2switch']
       test_sections:
           - default:
               - execute:
                   device: l2switch
                   command: ['show ip route']
               - configure:
                   device: l2switch
                   command: ['logging console']
               - execute:
                   device: l2switch
                   command: ['show ip route']
                   include:
                       - 'C        5.5.5.5 is directly connected, Loopback0'

Adding the file 'TC1.yaml' to pyATS *main_trigger_datafile* will make the
testcase part of an AUT script.

This concludes the basic usage of LAUT to generate AUT scripts while subsequently
doing UT.
