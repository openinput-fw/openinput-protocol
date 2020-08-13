=========================
OpenInput Device Protocol
=========================

The OpenInput protocol defines several functions to interact with the device.
Functions are organized in function pages to make it easier capability
discovery. It also defines several report types, with varying lenghts and
structure, to communicate with the device.

Each report type defines at least the following fields:

- Function Page
- Function ID
- Data

None of these fields has a specified size, reports types have total liberty over
how to represent this data over the wire.

Not chosing a size for the function page and ID was a deliberate decision, to
allow them to be expanded in the future if needed.


.. note::
    Function pages and function IDs **shall not** be joined together into one
    numeric representation. These fields don't have a specific size, and as
    such, they can't be safely represented as one parameter.


Report types
~~~~~~~~~~~~


Normal (Short and Long)
-----------------------


.. table:: Table 1 - Report structure

    +-------------+-----------+---------------+-------------+-----------+
    |     byte    |     0     |       1       |      2      | 3 .. End  |
    +=============+===========+===============+=============+===========+
    | description | Report ID | Function Page | Function ID |    Data   |
    +-------------+-----------+---------------+-------------+-----------+


Report ID
    The first byte of the report as defined in the HID specification. Supported
    report types by the device can be found in the HID report descriptor.

    There following report types are supported:

    .. table:: Table 2 - Supported report types

        =========== ======== ========
        Report Type   Value   Lenght
        =========== ======== ========
           Short    ``0x20``  7 bytes
           Long     ``0x21`` 19 bytes
        =========== ======== ========

Function Page
    This parameter represents the function page. It is unsigned and limited to a 
    maximum value of ``0xFF``.

Function ID
    This parameter represents the function ID. It is unsigned and limited to a 
    maximum value of ``0xFF``.

Data
    This parameter holds arbitrary data to interact with the functions. For
    requests they hold function arguments, and for responses, they hold return
    values.


.. table:: Table 4 - Short packet structure

    +-------------+------+---------------+-------------+---+---+---+---+
    |     byte    |   0  |       1       |      2      | 3 | 4 | 5 | 6 |
    +=============+======+===============+=============+===+===+===+===+
    | description | 0x20 | Function Page | Function ID |      Data     |
    +-------------+------+---------------+-------------+---------------+


.. table:: Table 5 - Long packet structure

    +-------------+------+---------------+-------------+---+---+---+---+---+---+---+----+----+----+----+----+----+----+----+----+
    |     byte    |   0  |       1       |      2      | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 |
    +=============+======+===============+=============+===+===+===+===+===+===+===+====+====+====+====+====+====+====+====+====+
    | description | 0x21 | Function Page | Function ID |                                 Data                                   |
    +-------------+------+---------------+-------------+------------------------------------------------------------------------+



Function pages
~~~~~~~~~~~~~~

- ``0x00`` Info (Protocol)
- ``0x01`` General Profiles
- ``0xFE`` Gimmicks
- ``0xFF`` Debug
