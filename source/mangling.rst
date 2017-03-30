GL Function Name Mangling
=========================

If you want to use both Mesa and another OpenGL library in the same
application at the same time you may find it useful to compile Mesa with
*name mangling*. This results in all the Mesa functions being prefixed
with **mgl** instead of **gl**.

This option is supported only with the autoconf build. To use it add
``--enable-mangling`` to your configure line.

.. code-block:: bash

    ./configure --enable-mangling ...
