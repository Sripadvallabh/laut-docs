device
======

Makes a particular device in the testbed active in LAUT shell.

Any operator command such as ``execute``, ``configure`` etc. only
work on the device loaded in the shell using the ``device`` command. Hence,
this command is primarily used to switch between multiple devices
in a testbed.

To know which device is currently loaded & active with respect to the
shell, LAUT shell prompt will always reflect the device currently *active*
in the shell in the format ``(lӓut-<ACTIVE_DEVICE_HOSTNAME>)``.

For example, switch from device 'host1' to 'host2' by giving the command
``device host2`` & notice that the prompt would change to *'(lӓut-host2)'*:

.. code-block:: console

   (lӓut-host1) device host2
   (lӓut-host2)

.. note::

   When a testbed is loaded on LAUT shell using ``testbed`` command, the first device mentioned in the testbed
   YAML is automatically loaded on the shell & becomes the active device w.r.t the shell.
