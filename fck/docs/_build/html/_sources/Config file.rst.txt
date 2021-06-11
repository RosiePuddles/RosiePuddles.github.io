Config file
===========

fck has a config file(named ``.fck`` and placed in the users root directory), containing values that determine how fck looks and feels. The general form for each line in the config file is as follows: ::

  <key>=<value>

If this form is not followed for a line, the line is skipped. A comprehensive list of all keys for the config file are given below

* ``wrapLength`` (integer)
    Maximum wrap length for the warning and error messages. Has a minimum value of 25 and no maximum value. Please don't abuse that though. I'd prefer not to have to deal with some obscure error from someone having a wrap length of 1000.
* ``lang`` (string)
    This is the path to the syntax language file used by fck. Full documentation on language specific syntax is provided :doc:`here </Language specific syntax>`.
