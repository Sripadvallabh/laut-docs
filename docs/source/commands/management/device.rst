
.. _device:

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

For example, switch from device 'leaf2-lag2' to 'l2switch' by giving the command
``device l2switch`` & notice that the prompt would change to *'(lӓut-l2switch)'*:

.. code-block:: console

    (lӓut-leaf2-lag2) device l2switch
    (lӓut-l2switch)

.. note::

   When a testbed is loaded on LAUT shell using ``testbed`` command, the first device mentioned in the testbed
   YAML is automatically loaded on the shell & becomes the active device w.r.t the shell.
