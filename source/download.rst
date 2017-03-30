Downloading / Unpacking
=======================

Primary Mesa download site:
`ftp.freedesktop.org <ftp://ftp.freedesktop.org/pub/mesa/>`__ (FTP) or
`mesa.freedesktop.org <https://mesa.freedesktop.org/archive/>`__ (HTTP).

Starting with the first release of 2017, Mesa's version scheme is
year-based. Filenames are in the form mesa-Y.N.P.tar.gz, where Y is the
year (two digits), N is an incremental number (starting at 0) and P is
the patch number (0 for the first release, 1 for the first patch after
that).

When a new release is coming, release candidates (betas) may be found in
the same directory, and are recognisable by the mesa-Y.N.P-rcX.tar.gz
filename.

Unpacking
---------

Mesa releases are available in two formats: .tar.xz and .tar.gz.

To unpack the tarball:

.. code-block:: bash

   tar xf mesa-Y.N.P.tar.xz

or

.. code-block:: bash

   tar xf mesa-Y.N.P.tar.gz

Contents
~~~~~~~~

After unpacking you'll have these files and directories (among others):

.. code-block:: text

    autogen.sh  - Autoconf script for *nix systems
    scons/      - SCons script for Windows builds
    include/    - GL header (include) files
    bin/        - shell scripts for making shared libraries, etc
    docs/       - documentation
    src/        - source code for libraries
    src/mesa    - sources for the main Mesa library and device drivers
    src/gallium - sources for Gallium and Gallium drivers
    src/glx     - sources for building libGL with full GLX and DRI support

Proceed to the `compilation and installation instructions <install.html>`__.

Demos, GLUT, and GLU
--------------------

A package of SGI's GLU library is available `here <ftp://ftp.freedesktop.org/pub/mesa/glu/>`__

A package of Mark Kilgard's GLUT library is available `here <ftp://ftp.freedesktop.org/pub/mesa/glut/>`__

The Mesa demos collection is available `here <ftp://ftp.freedesktop.org/pub/mesa/demos/>`__

In the past, GLUT, GLU and the Mesa demos were released in conjunction
with Mesa releases. But since GLUT, GLU and the demos change
infrequently, they were split off into their own git repositories:
`GLUT <https://cgit.freedesktop.org/mesa/glut/>`__,
`GLU <https://cgit.freedesktop.org/mesa/glu/>`__ and
`Demos <https://cgit.freedesktop.org/mesa/demos/>`__.
