Built-in types
==============

This page details the different built in types within fck. For the different types of operators used by fck, please see the :doc:`operators </Operators>` page.

Each section for the different types follows a general structure:

* | **General overview and variable creation**
  | Gives a general overview of the value type, how to define it, what it inherits from to show inherited methods, and what it holds

* | **Casting value processing**
  | Details the process to cast any type as the current type. All warnings listed in these sections are only raised when the original type is not implicitly cast, such as passing into functions, and not when using the ``as`` keyword

* | **Illegal operations**
  | Gives a list of the operators that cannot be used with the given type and details why. Also gives cases where normally allowed operators are illegal. This section may be omitted if it is not required for the given type

* | **Other sections relating to the type**
  | Many types have specific sections pertaining to specific details about that type, such as escape characters for the ``str`` type

.. _int type:

``int`` type
------------
.. code-block:: fckREGEX

    int <identifier> ((::|:>) <value>)?

The ``int`` variable type inherits from the ``Number`` class, and holds a single integer value. It has a default value of 0.

Casting value processing
^^^^^^^^^^^^^^^^^^^^^^^^

+---------------+------------------------------------------------------------+--------------------------+
| Value type    | Processing                                                 | Warning or error raised  |
+---------------+------------------------------------------------------------+--------------------------+
| ``int``       | None                                                       | None                     |
+---------------+------------------------------------------------------------+--------------------------+
| ``float``     | Value is rounded                                           | None                     |
+---------------+------------------------------------------------------------+--------------------------+
| ``list``      || **For a list with a single element in recursively:**      | :ref:`W014<warnings>`    |
|               || The single element is removed and processed depending on  |                          |
|               | its type                                                   |                          |
|               +------------------------------------------------------------+--------------------------+
|               || **For all other lists:**                                  | :ref:`E000<errors>`      |
|               || An error is raised                                        |                          |
+---------------+------------------------------------------------------------+--------------------------+
| ``str``       | fck will attempt to convert the string into a base 10      || Warning:                |
|               | integer, or float and round the value. If it is not        | :ref:`W016<warnings>`    |
|               | successful, an error is raised. If it is successful, a     || Error:                  |
|               | warning is raised.                                         | :ref:`E000<errors>`      |
+---------------+------------------------------------------------------------+--------------------------+

``float`` type
--------------
.. code-block:: fckREGEX

    float <identifier> ((::|:>) <value>)?

The ``float`` variable type inherits from the ``Number`` class, and holds a single floating point value. It has a default value of 0.0.

Casting value processing
^^^^^^^^^^^^^^^^^^^^^^^^

+---------------+------------------------------------------------------------+--------------------------+
| Value type    | Processing                                                 | Warning or error raised  |
+---------------+------------------------------------------------------------+--------------------------+
| ``int``       | Converts integer value to floating point value             | None                     |
+---------------+------------------------------------------------------------+--------------------------+
| ``float``     | None                                                       | None                     |
+---------------+------------------------------------------------------------+--------------------------+
| ``list``      || **For a list with a single element in recursively:**      | :ref:`W014<warnings>`    |
|               || The single element is removed and processed depending on  |                          |
|               | its type                                                   |                          |
|               +------------------------------------------------------------+--------------------------+
|               || **For all other lists:**                                  | :ref:`E000<errors>`      |
|               || An error is raised                                        |                          |
+---------------+------------------------------------------------------------+--------------------------+
| ``str``       | fck will attempt to convert the string into a base 10      || Warning:                |
|               | float. If it is not                                        | :ref:`W016<warnings>`    |
|               | successful, an error is raised. If it is successful, a     || Error:                  |
|               | warning is raised                                          | :ref:`E000<errors>`      |
+---------------+------------------------------------------------------------+--------------------------+

``str`` type
------------
.. code-block:: fckREGEX

    str <identifier> ((::|:>) <value>)?

The ``str`` type inherits from the ``Value`` class and contains a single string, which can be empty. The default value is an empty string(``''``). To create a string, the value has to be surrounded by a pair of either inverted commas(``'``) or quotation marks(``"``).

Casting value processing
^^^^^^^^^^^^^^^^^^^^^^^^

+---------------+------------------------------------------------------------+--------------------------+
| Value type    | Processing                                                 | Warning or error raised  |
+---------------+------------------------------------------------------------+--------------------------+
| ``int``       | If the value can be converted into a float with base 10,   | None                     |
|               | then value is converted and rounded                        |                          |
|               +------------------------------------------------------------+--------------------------+
|               | If the value cannot be converted into a base 10 float,     | :ref:`E006<errors>`      |
|               | then an error is raised                                    |                          |
+---------------+------------------------------------------------------------+--------------------------+
| ``float``     | Uses the same processing as for an ``int``, but omits      | See above                |
|               | rounding the values                                        |                          |
+---------------+------------------------------------------------------------+--------------------------+
| ``list``      | Returns a list with the string as the only element         | None                     |
+---------------+------------------------------------------------------------+--------------------------+
| ``str``       | None                                                       | None                     |
+---------------+------------------------------------------------------------+--------------------------+

Escape characters
^^^^^^^^^^^^^^^^^

In fck, the forward slash ``\`` is used. The character after a ``\`` can ba anything and is added to the string regardless; however there are a few characters after a forward slash that have special meanings. These are listed below:

+--------------+---------------------------------+
| Character    | Description                     |
+--------------+---------------------------------+
| ``\n``       | New line                        |
+--------------+---------------------------------+
| ``\t``       | Tab                             |
+--------------+---------------------------------+



.. _list documentation:

``list`` type
-------------
.. code-block:: fckREGEX

    list <identifier> ((::|:>) <value>)?

The ``list`` type inherits from the ``Value`` class and contains a list of elements, which can be empty. The default value is an empty list(``[]``). When a value is assigned to a ``list`` type variable or cast as a ``list`` using the ``as`` keyword, some preprocessing may be required depending on the type of value being assigned. This preprocessing is outlined in the table below:

+---------------+------------------------------------------------------------+--------------------------+
| Value type    | Processing                                                 | Warning or error raised  |
+---------------+------------------------------------------------------------+--------------------------+
| ``int``,      | Value is put into a list, and a warning is raised          | :ref:`W008<warnings>`    |
| ``float``, or |                                                            |                          |
| ``str``       |                                                            |                          |
+---------------+------------------------------------------------------------+--------------------------+
| ``list``      | None                                                       | None                     |
+---------------+------------------------------------------------------------+--------------------------+

A ``list`` type can be multi-dimensional.

Accessing element or slice of a list
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: fckREGEX

    <list to access>[<range>(,<range>)*]

..

.. code-block:: fckREGEX

    <first index>..<second index> | <single index>

When accessing a list, you can either access a slice or single index using a slice range or element range respectively..

When accessing a slice, one of the first or second index must be specified. The first value defaults to the start of the list, and the second value defaults to the end of the list. If either of the given indices aren't ``int`` types, they are converted into ``int`` types [1]_. If this is successful, the values are then compared against the length of the list. If the indices referenced are outside the length of the list, a warning is raised(:ref:`W009<warnings>`) and the value is altered to fit within the length of the list. After this has happened, the range is retrieved from the list. If the first index is further through the list than the second value, the returned slice is reversed. So a range of ``5..1`` would return a slice containing elements 5, 4, 3, and 2 in that order. The second index is the stopping index, and is not included in a slice.

When accessing a single index, a single value is given, which is then converted to an int [1]_, and clamped to the within the length of the list.

To access a value or slice in a multi-dimensional list, multiple ranges can be referenced, separated with a comma. For a list reference with multiple ranges, only the last range can be a slice range, so the compound range ``1..2, 3`` is not allowed for a list [2]_, but ``1, 2..3`` is.

Indices can also be negative. A negative index refers to the n:superscript:`th` last element. So the range ``..-1`` would return a slice containing the entire list except the last item. Negative indices can be used for either the first or second index in a slice range, or for an element range.

.. [1] See `int type`_ reference for more details on conversion into ``int`` types.
.. [2] This is allowed for an ``array`` of type ``list`` however. This is not allowed for lists since a list can contain several different data types, some of which a slice based range cannot be applied to.

.. _list recursion:

Single and zero element recursion
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In fck, lists have properties known as single and zero element recursion. Technically, a list has single element recursion if it recursively only contains one element, and zero element recursion if it recursively contains no elements. In practice, this means that a list can have a single nested list with one element in, giving the list single element recursion. For example the list ``[1]`` has single element recursion, as does ``[[[['hello']]]]``, but ``[[1], 2]`` does not, since there is more than one value contained within the parent list, and all nested lists. This is evaluated in the same way for zero element recursion, but the requirement is that no elements are contained within the parent list or any nested lists.

Single element recursion is used when casting a list as another type. If a list with single element recursion is cast as a single value type(``int``, ``float``, ``str``, or ``bool``) the single element in the list is cast as the given type. For example ``[[['1']]] as int`` would return the same as ``'1' as int``.

Despite being stated as a single value type above, the ``bool`` type does not follow this method. When casting a ``list`` as a ``bool``, the returned value is ``true`` in all cases except if the list has zero element recursion.

If a list with zero element recursion is cast as a single value type, the default value is used.

When casting a list to a single value type, a warning(:ref:`W014<warnings>`) is raised.


.. _auto type:

``auto`` type
-------------
.. code-block:: fckREGEX

    auto <identifier> (::|:>) <value>

The ``auto`` type, whilst not a type in itself, is a variable decleration keyword that does not require the user to know the type of value being assigned.

The order of preference for the ``auto`` type is as follows for single values:

#. ``float``
#. ``int``
#. ``str``
#. ``bool``

For multiple value types:

#. ``array``
#. ``vector`` only if curved bracket(``(``) delimeters are used and is one dimensional
#. ``list``
#. ``dict``

.. _null type:

``null`` type
-------------

Built-in values
---------------

fck has a few built-in values which cannot be altered. These are given below:

+-------------+------------------+---------------------------------------------------------------+
| Identifier  | Type             | Explanation                                                   |
+-------------+------------------+---------------------------------------------------------------+
| ``true``    | ``bool``         | Returned value when a comparison is true                      |
+-------------+------------------+---------------------------------------------------------------+
| ``false``   | ``bool``         | Returned value when a comparison is false                     |
+-------------+------------------+---------------------------------------------------------------+
| ``null``    | ``null``         | Basic type that contains no value. :ref:`Reference<null type>`|
+-------------+------------------+---------------------------------------------------------------+
