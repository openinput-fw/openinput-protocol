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
           Short    ``0x20``  8 bytes
           Long     ``0x21`` 32 bytes
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

    +-------------+------+---------------+-------------+---+---+---+---+---+
    |     byte    |   0  |       1       |      2      | 3 | 4 | 5 | 6 | 7 |
    +=============+======+===============+=============+===+===+===+===+===+
    | description | 0x20 | Function Page | Function ID |        Data       |
    +-------------+------+---------------+-------------+-------------------+


.. table:: Table 5 - Long packet structure

    +-------------+------+---------------+-------------+---------+
    |     byte    |   0  |       1       |      2      | 3 .. 31 |
    +=============+======+===============+=============+=========+
    | description | 0x21 | Function Page | Function ID |   Data  |
    +-------------+------+---------------+-------------+---------+


Sending reports
~~~~~~~~~~~~~~~

Request
-------

You may use one of the supported report IDs to communicate with the device.
Function parameters are passes in the data section and their format is defined
by the function specification. Unused data sections should be set to 0. Unused
data sections from reports coming from the device should be ignored.

Reply
-----

The device will reply with one of the report types, it doesn't need to be the
same used in the request.
The function page and ID will be set to the same value as the request, unless
there was an error.

Errors
......

If there was an error processing the request, the function page will be set to
the `Error` function page ID (``0xFF``) and the function ID to the error ID, as
defined by the `Error` function page. The first two bytes of the data segment
will hold the function page and ID of the quest that triggered the error.
The remaining of the data might be used by the error to pass arguments.

.. table:: Table 6 - Short error reply packet structure

    +-------------+------+----------------------------+----------+---------------+-------------+-----------------+
    |     byte    |   0  |              1             |     2    |       3       |      4      |      5 .. 7     |
    +=============+======+============================+==========+===============+=============+=================+
    | description | 0x20 | 0xFF (Error Function Page) | Error ID | Function Page | Function ID | Error Arguments |
    +-------------+------+----------------------------+----------+---------------+-------------+-----------------+


Mandatory Functions
~~~~~~~~~~~~~~~~~~~

The following functions are mandatory, all devices that comply with this
specification should support them.

- Info (``0x00``), Version (``0x00``)
- Info (``0x00``), Firmware Information (``0x01``)
- Info (``0x00``), Supported Function Pages (``0x02``)
- Info (``0x00``), Supported Functions (``0x03``)


Function pages
~~~~~~~~~~~~~~

- ``0x00`` Info (Protocol)
- ``0x01`` General Profiles
- ``0xFD`` Gimmicks
- ``0xFE`` Debug
- ``0xFF`` *Error* (special, see `Errors` section)


Notes
~~~~~

All values and field values specified here and in the function definitions are
assumed to be unsigned unless specified oitherwise.
