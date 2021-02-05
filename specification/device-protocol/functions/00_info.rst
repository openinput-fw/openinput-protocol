=============
``0x00`` Info
=============

This function page defines functions to get information from the device.


Functions
~~~~~~~~~


``0x00`` Version
----------------

This function returns the implemented protocol version.


Request
.......

No arguments.


Reply
.....

Protocol major, minor and patch versions.

.. table:: Table 1 - Version return argument structure

    +-------------+-----------+-----------+-----------+----------+
    |     byte    |     0     |     1     |     2     | 3 .. End |
    +=============+===========+===========+===========+==========+
    | description | ``major`` | ``minor`` | ``patch`` |          |
    +-------------+-----------+-----------+-----------+----------+


``0x01`` Firmware Information
-----------------------------

This function returns several pieces of information about the firmware.


Request
.......


.. table:: Table 2 - Field IDs

    ======== ================
       ID       Description
    ======== ================
    ``0x00`` Firmware vendor
    ``0x01`` Firmware version
    ======== ================


.. table:: Table 3 - Firmware information request argument structure

    +-------------+-----------+----------+
    |     byte    |     0     | 2 .. End |
    +=============+===========+==========+
    | description |   ``id``  |          |
    +-------------+-----------+----------+


Reply
.....

A description of the requested field. The description is comprised of ascii
characters terminated by either the null character or the end of the packet.

.. table:: Table 4 - Firmware information argument structure

    +-------------+--------------------+
    |     byte    |      0 .. End      |
    +=============+====================+
    | description | (string) ``value`` |
    +-------------+--------------------+
