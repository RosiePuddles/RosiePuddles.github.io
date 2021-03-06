.. role:: warning

Built-in methods
================

This page details all the different type of built-in methods in fck. Several of these methods rely on condition evaluation to check if a given suite should be executed. The condition evaluation in fck is different to most other coding languages and is detailed `here`_.

While loop
----------
.. code-block:: fckREGEX

    while <condition> {
        <suite>
    }

The ``while`` loop executes a given suite while the given condition is true. ``while`` loops can be named. See the naming methods section for how to `name methods`_.

Iterate loop
------------
.. code-block:: fckREGEX

    iterate <range> {
        <suite>
    }

The ``iterate`` loop executes a given suite a given number of times. As with ``while`` loops, ``iterate`` loops can be named. See the naming methods section for how to `name methods`_.

Range based iteration
^^^^^^^^^^^^^^^^^^^^^
.. code-block:: fckREGEX

   (<first value> to)? <second value> (step <step value>)?  (:: <iterable variable>)?

The ``range`` of an iterate loop is very flexible and has several optional parts for range based iteration.

If a first value is not given, this is assumed to be 0. The first value is the starting value, and the iteration ends when the current value is larger than or equal to the second value if the step value is positive, and less than or equal to the second value if the step value is negative. The step value can be any finite value, positive or negative. If the step value is given as 0, a warning is raised and the step value defaults back to ±1 depending on the value of the second value in relation to the first value.

The iterable variable is a temporary variable assigned to the current value being iterated over at the start of each iteration. This value **can** be overwritten during the iteration and is not rewritten by the iterate loop. Once the iterate loop has finished this variable is removed from the global scope.

Due to the flexibility and design principles of fck, there are several cases in which warnings can be raised. These are given below:

+------------------------------------+-----------------------------------------------------------------+
|Case                                |What fck does to fix this                                        |
+------------------------------------+-----------------------------------------------------------------+
|First value is lower than the       |The step value is made positive                                  |
|second value and the step value is  |                                                                 |
|negative                            |                                                                 |
+------------------------------------+-----------------------------------------------------------------+
|Second value is lower than the      |The step value is made negative                                  |
|first value and the step value is   |                                                                 |
|positive                            |                                                                 |
+------------------------------------+-----------------------------------------------------------------+
|Step value is 0                     |The step value is set to 1 if the second value is larger than    |
|                                    |the first value, else -1                                         |
+------------------------------------+-----------------------------------------------------------------+

List, array, or dictionary based iteration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: fckREGEX

    <list, array, or dict> (step <step value>)? (:: <iterable variable>)?

The range parameter of the ``iterate`` method can also be an ``array``, ``list``, or ``dict``, with the step value and iterable variable being used the same as for the range based

If statement
------------
.. code-block:: fckREGEX

    if <condition> {
        <suite>
    }
    (elif <condition> {
        <suite>
    })*
    (else {
        <suite>
    })?

The ``if`` statement runs a given suite of code is the related condition is evaluated to ``true``. If not, then the statement moves through the ``elif`` statements, if there are any, and evaluates the suite of the ``elif`` statement if its related condition evaluates to ``true``. Once all ``elif`` statements have been checked, the ``else`` statement is executed if there is an ``else`` statement.

Once any of the statements (``if``, ``elif`` or ``else``) have been executed, no more statements are considered and the statement finishes evaluation.

Case statement
--------------
.. code-block:: fckREGEX

   case <expression> {
      (option <expression> {
         <suite>
      })*
      (default {
         <suite>
      })?
   }

The ``case`` statement evaluates a given expression, and then iterates over all of the ``option`` statements, evaluating the related expression and checking if the ``option`` expression matches the ``case`` expression. If it does then the related suite is executed. Once all ``option`` statements have been checked against, the ``default`` statements suite is evaluated, if there is a ``default`` statement.

Similarly to the ``if`` statement, if any of the ``option`` statements suites are executed, the ``case`` statement doesn't check and further ``option`` statements or the ``default`` statement and finishes evaluation.


``?`` operator
--------------
.. code-block:: fckREGEX

    <condition to evaluate> ? <expression to return if true> : <expression to return if false>

The ``?`` operator can be used as an in-line if else statement. At least one of the expressions is required, but not both of them [#]_. When only one is used the ``:`` must be left in or an error will be raised. When an expression is left empty, a ``null`` value is returned.

These in-line if else statements can be nested to create if elif else statements.

.. [#] Using the previously established methods of showing this(taken from regex) has been excluded to avoid confusion

.. _name methods:

Method naming
-------------

In fck, methods can be named for use in conjunction with the ``break`` and ``continue`` keywords. To name a method, the ``@`` character is used followed by the name, and then the
method. For example:

.. code-block:: fckREGEX

    @loop while <condition> {
        <suite>
    }

If multiple methods are given the same name, these names will be altered with the suffix of 1, 2, 3 etc. assuming that these names do not exist either, with the exception of the first name, which will be left unaltered. :warning:`This will raise a warning.`

.. _here:

Condition evaluation
--------------------

Conditions are evaluated differently in fck when compared to many coding languages. The table below details how different types of conditions are evaluated:

============== ================================
Condition type How is it evaluated to a boolean
============== ================================
Numerical      If the value is above 0, the
               condition evaluates to ``true``
``string``     If the length of the string is
               0 it evaluates to ``false``
``list``       If the length contains no values
               (or recursively contains a list with no items in)
               it evaluates to ``false``
============== ================================

The ``list`` is a slightly confusing entry since it's a fairly complex evaluation. When a list is evaluated as a condition, if the list has nothing in, it returns ``false``. If the list **only** has  a list inside it, ths list is evaluated instead. If neither of these are true then the list is evaluated as ``true``. See below for some examples:


=================== ==========
List                Evaluation
=================== ==========
``[]``              ``false``
``[[]]``            ``false``
``[[], 1]``         ``true``
``[5]``             ``true``
``["hello world"]`` ``true``
=================== ==========
