Language specific syntax
------------------------

fck supports language specific syntax. What this means is that fck is bundles with several files for syntax in different languages, with more being added with each version. If your language is missing, feel free to request it or help out by making one yourself for your language.
A default language can be specified in the ``.fck`` config file and defaults to ``/fck/en``, or alternatively specified at the start of the script using the following syntax:

.. code-block:: fckREGEX

   # /fck/<language code>!

The ``<language code>`` can be any of the following (taken from ISO 639-1):

* ``en`` English
* ``fr`` French
* ``es`` Spanish
* ``de`` German
* ``it`` Italian
* ``no`` Norwegian
* ``sv`` Swedish
