Custom Functions
================

This page details all the different methods associated with custom functions. in fck.

.. _funcDef:

Function defenition
-------------------
.. code-block:: fckREGEX

    def <function identifier>? (<function parameters>*) (-> <return type>)?{
        <function suite>
    }

To define a function, the keyword ``def`` is used. Functions do not have to have predefined return types, but can optionally be specified with an arrow followed by the return type.

Identifier
----------

The function identifier can contain any mixture of upper and lower case letters, numbers, and underscores. The function identifier name must match the regex pattern ``\w+`` or ``[A-Za-z0-9_]+``.

Identifiers are optional however. When the function is not given a name, the function has the name ``<anonymous>`` and cannot be called.

Predefined return types
-----------------------

By default, functions have no predefined return type; however, one can be speified. This specification comes after the arguments and is a type or list of type after an arrow. For example, the following function takes in two floats, and returns an int:

.. code-block:: fckREGEX

    def add(float a, float b) -> int{
      return (a + b) as int
    }

If a return type is specified, and then a value is returned that does not have a matching type, the vaue is cast as the defined return type if possible, and a warning is raised. This means that the example above can be condensed to the following:

.. code-block:: fckREGEX

    def add(float a, float b) -> int{
      return a + b
    }

Please note however, if the returned value cannot be cast as the defined type, then an error is raised.

Arguments
---------

When a function is defined, the arguments are defined within brackets after the function identifier. Arguments have to be specified by type. Each argument must follow the pattern ``<type of variable> <variable identifier> (:: <default value>)?``. Arguments with a default variable can be included at any point in the list of arguments. For example, both of the following are valid function definitions:

.. code-block:: fckREGEX

   def add(int a, int b :: 1){<suite>}
   def add(int a :: 1, int b){<suite>}

Passing in arguments
~~~~~~~~~~~~~~~~~~~~

When passing in arguments into a function, the number of arguments will dictate which arguments is filled or altered. To example this, the following example function will be used:

.. code-block:: fckREGEX

   def func(str a, int b :: 1, float c, list d, list e:: [[2, 4], [1, 9]]){
        <suite>
   }

This function has a total of 5 arguments, 3 without defaults (compulsory), and 2 with defaults (optional). When calling this function, if three values are passed in without an argument identifier prefixing it, then these will be assigned to ``a``, ``c``, and ``d``, with each value first being cast as the type of each argument. So the first argument would be cast as a ``str`` type and so on. If 4 values were given as arguments, then ``a``, ``b``, and ``d`` would still be filed since they are compulsory, but ``b`` would be altered from its default value. Passing in 4 arguments would also mean that the values previously assigned to ``c`` and ``d`` would be different. The following example explains this:

.. code-block:: fckREGEX

   ## a  'hello world!'
   ## b  1
   ## c  3.14159
   ## d  [1, 2, 3]
   ## e  [[2, 4], [1, 9]]
   func('hello world!', 3.14159, [1, 2, 3])
