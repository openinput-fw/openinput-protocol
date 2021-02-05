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

