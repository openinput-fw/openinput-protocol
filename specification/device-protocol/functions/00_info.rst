=============
``0x00`` Info
=============

This function page defines functions to get information from the device.


.. toctree::


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
    ``0x02`` Device name
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


``0x02`` Supported Function Pages
----------------------------------

This function returns the list of supported function pages by the device.


Request
.......

Because a single report might not be enough to get the full list, the list may
have to by fetched by parts. For this reason the function supports getting the
list starting at a certain index, which is indicated by the ``start_index``
argument.

The first time you call this, you will probably want to set ``start_index`` to 0
and then re-call the function with an incremented index if there are still
elements left in the list.

.. table:: Table 5 - Supported function pages request argument structure

    +-------------+-----------------+----------+
    |     byte    |         0       | 1 .. End |
    +=============+=================+==========+
    | description | ``start_index`` |          |
    +-------------+-----------------+----------+


Reply
.....

The reply contains the number of elements returned, the number of elements left
and the list elements.

.. table:: Table 6 - Supported functions return argument structure

    +-------------+-----------+------------------------+------------------+
    |     byte    |     0     |            1           |     2 .. End     |
    +=============+===========+========================+==================+
    | description | ``count`` | ``remaining_elements`` | ``element_list`` |
    +-------------+-----------+------------------------+------------------+


``0x03`` Supported Functions
----------------------------

This function returns the list of supported functions by the device.

This function is very similar to the ``0x02`` *Supported Function Pages*
function, the only difference being the addition of a ``function_page``
argument to the request.


Request
.......

Because a single report might not be enough to get the full list, the list may
have to by fetched by parts. For this reason the function supports getting the
list starting at a certain index, which is indicated by the ``start_index``
argument.

The first time you call this, you will probably want to set ``start_index`` to 0
and then re-call the function with an incremented index if there are still
elements left in the list.

.. table:: Table 6 - Supported functions request argument structure

    +-------------+-------------------+-----------------+----------+
    |     byte    |         0         |         1       | 2 .. End |
    +=============+===================+=================+==========+
    | description | ``function_page`` | ``start_index`` |          |
    +-------------+-------------------+-----------------+----------+


Reply
.....

The reply contains the number of elements returned, the number of elements left
and the list elements.

.. table:: Table 7 - Supported functions return argument structure

    +-------------+-----------+------------------------+------------------+
    |     byte    |     0     |            1           |     2 .. End     |
    +=============+===========+========================+==================+
    | description | ``count`` | ``remaining_elements`` | ``element_list`` |
    +-------------+-----------+------------------------+------------------+
