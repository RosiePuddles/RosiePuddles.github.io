Built-in functions
==================

``print`` function
------------------
.. code-block:: fck

  def print(str value :: '') -> null{}

Prints the given value to the console with a new line at the end. The printed string has string formatting.

``log`` function
----------------
.. code-block:: fck

  def log(str value :: '') -> null{}

Prints the given string to the console without a new line. The printed string does not have string formatting enabled. The ``log`` function can be disabled globally with the global option ```nolog``.

``type`` function
-----------------
.. code-block:: fck

  def type(auto value) -> str{}

Returns the type of the given value as a string.

``input`` function
------------------
.. code-block:: fck

  def input(str prompt :: '') -> str{}

Gets a user input and returns it as a string.

``run`` function
----------------
.. code-block:: fck

  def run(str fn) -> null{}

Run a fck script from inside another fck script.

``quit`` function
-----------------
.. code-block:: fck

  def quit(int exit_code :: 0) -> null{}

Exits the script entirely with a given exit code.
