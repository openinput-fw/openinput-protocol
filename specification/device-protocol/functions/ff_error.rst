==============
``0xFF`` Error
==============

.. toctree::


``0x01`` Invalid value
-------------------------

This error indicates that the value of an argument is invalid.


Arguments
.........

``position`` points to the position of the first byte of the argument that
caused the error.

.. table:: Table 1 - Inavalid value error argument structure

    +-------------+--------------+----------+
    |     byte    |       0      | 1 .. End |
    +=============+==============+==========+
    | description | ``position`` |          |
    +-------------+--------------+----------+


``0x02`` Unsupported function
-----------------------------

This error indicates that the function requested is not supported.


``0xFE`` Custom error
---------------------

Arguments
.........

``description`` is comprised of ascii characters terminated by either the null
character or the end of the packet.

.. table:: Table 2 - Custom error argument structure

    +-------------+-----------------+
    |     byte    |     0 .. End    |
    +=============+=================+
    | description | ``description`` |
    +-------------+-----------------+
