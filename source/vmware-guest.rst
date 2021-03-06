VMware guest GL driver
======================

This page describes how to build, install and use the
`VMware <https://www.vmware.com/>`__ guest GL driver (aka the SVGA or
SVGA3D driver) for Linux using the latest source code. This driver gives
a Linux virtual machine access to the host's GPU for
hardware-accelerated 3D. VMware Workstation running on Linux or Windows
and VMware Fusion running on MacOS are all supported.

With the August 2015 Workstation 12 / Fusion 8 releases, OpenGL 3.3 is
supported in the guest. This requires:

-  The VM is configured for virtual hardware version 12.
-  The host OS, GPU and graphics driver supports DX11 (Windows) or
   OpenGL 4.0 (Linux, Mac)
-  On Linux, the vmwgfx kernel module must be version 2.9.0 or later.
-  A recent version of Mesa with the updated svga gallium driver.

Otherwise, OpenGL 2.1 is supported.

OpenGL 3.3 support can be disabled by setting the environment variable
SVGA\_VGPU10=0. You will then have OpenGL 2.1 support. This may be
useful to work around application bugs (such as incorrect use of the
OpenGL 3.x core profile).

Most modern Linux distros include the SVGA3D driver so end users
shouldn't be concerned with this information. But if your distro lacks
the driver or you want to update to the latest code these instructions
explain what to do.

For more information about the X components see these wiki pages at
x.org:

-  `Driver Overview <https://wiki.x.org/wiki/vmware>`__
-  `xf86-video-vmware
   Details <https://wiki.x.org/wiki/vmware/vmware3D>`__

Components
----------

The components involved in this include:

-  Linux kernel module: vmwgfx
-  X server 2D driver: xf86-video-vmware
-  User-space libdrm library
-  Mesa/gallium OpenGL driver: "svga"

All of these components reside in the guest Linux virtual machine. On
the host, all you're doing is running VMware
`Workstation <https://www.vmware.com/products/workstation/>`__ or
`Fusion <https://www.vmware.com/products/fusion/>`__.

Prerequisites
-------------

-  Kernel version at least 2.6.25
-  Xserver version at least 1.7
-  Ubuntu: For ubuntu you need to install a number of build dependencies.

   .. code-block:: bash

      sudo apt-get install git-core
      sudo apt-get install automake libtool libpthread-stubs0-dev
      sudo apt-get install xserver-xorg-dev x11proto-xinerama-dev libx11-xcb-dev
      sudo apt-get install libxcb-glx0-dev libxrender-dev
      sudo apt-get build-dep libgl1-mesa-dri libxcb-glx0-dev

-  Fedora: For Fedora you also need to install a number of build dependencies.

   .. code-block:: bash

      sudo yum install mesa-libGL-devel xorg-x11-server-devel xorg-x11-util-macros
      sudo yum install libXrender-devel.i686
      sudo yum install automake gcc libtool expat-devel kernel-devel git-core
      sudo yum install makedepend flex bison

Depending on your Linux distro, other packages may be needed.
The configure scripts should tell you what's missing.

Getting the Latest Source Code
------------------------------

Begin by saving your current directory location:

.. code-block:: bash

    export TOP=$PWD

-  Mesa/Gallium master branch. This code is used to build libGL, and the
   direct rendering svga driver for libGL, vmwgfx\_dri.so, and the X
   acceleration library libxatracker.so.x.x.x.

   .. code-block:: bash

       git clone git://anongit.freedesktop.org/git/mesa/mesa

-  VMware Linux guest kernel module. Note that this repo contains the
   complete DRM and TTM code. The vmware-specific driver is really only
   the files prefixed with vmwgfx.

   .. code-block:: bash

      git clone git://anongit.freedesktop.org/git/mesa/vmwgfx

-  libdrm, a user-space library that interfaces with drm. Most distros
   ship with this but it's safest to install a newer version. To get the
   latest code from git:

   .. code-block:: bash

      git clone git://anongit.freedesktop.org/git/mesa/drm

-  xf86-video-vmware. The chainloading driver, vmware\_drv.so, the
   legacy driver vmwlegacy\_drv.so, and the vmwgfx driver
   vmwgfx\_drv.so.

   .. code-block:: bash

      git clone git://anongit.freedesktop.org/git/xorg/driver/xf86-video-vmware

Building the Code
-----------------

-  Determine where the GL-related libraries reside on your system and
   set the LIBDIR environment variable accordingly.

   For 32-bit Ubuntu systems:

   .. code-block:: bash

      export LIBDIR=/usr/lib/i386-linux-gnu

   For 64-bit Ubuntu systems:

   .. code-block:: bash

      export LIBDIR=/usr/lib/x86_64-linux-gnu

   For 32-bit Fedora systems:

   .. code-block:: bash

      export LIBDIR=/usr/lib

   For 64-bit Fedora systems:

   .. code-block:: bash

      export LIBDIR=/usr/lib64

-  Build libdrm:

   .. code-block:: bash

      cd $TOP/drm
      ./autogen.sh --prefix=/usr --libdir=${LIBDIR}
      make
      sudo make install

-  Build Mesa and the vmwgfx\_dri.so driver, the vmwgfx\_drv.so xorg
   driver, the X acceleration library libxatracker. The vmwgfx\_dri.so
   is used by the OpenGL libraries during direct rendering, and by the
   Xorg server during accelerated indirect GL rendering. The
   libxatracker library is used exclusively by the X server to do
   render, copy and video acceleration:

   The following configure options doesn't build the EGL system.

   .. code-block:: bash

      cd $TOP/mesa
      ./autogen.sh --prefix=/usr --libdir=${LIBDIR} --with-gallium-drivers=svga --with-dri-drivers=swrast --enable-xa --disable-dri3 --enable-glx-tls
      make
      sudo make install

   Note that you may have to install other packages that Mesa depends
   upon if they're not installed in your system. You should be told
   what's missing.

-  xf86-video-vmware: Now, once libxatracker is installed, we proceed
   with building and replacing the current Xorg driver. First check if
   your system is 32- or 64-bit.

   .. code-block:: bash

      cd $TOP/xf86-video-vmware
      ./autogen.sh --prefix=/usr --libdir=${LIBDIR}
      make
      sudo make install

-  vmwgfx kernel module. First make sure that any old version of this
   kernel module is removed from the system by issuing

   .. code-block:: bash

      sudo rm /lib/modules/`uname -r`/kernel/drivers/gpu/drm/vmwgfx.ko*

   Build and install:

   .. code-block:: bash

      cd $TOP/vmwgfx
      make
      sudo make install
      sudo depmod -a

   If you're using a Ubuntu OS:

   .. code-block:: bash

      sudo update-initramfs -u

   If you're using a Fedora OS:

   .. code-block:: bash

      sudo dracut --force

   Add 'vmwgfx' to the /etc/modules file:

   .. code-block:: bash

      echo vmwgfx | sudo tee -a /etc/modules

   Note: some distros put DRM kernel drivers in different directories.
   For example, sometimes vmwgfx.ko might be found in
   ``/lib/modules/{version}/extra/vmwgfx.ko`` or in
   ``/lib/modules/{version}/kernel/drivers/gpu/drm/vmwgfx/vmwgfx.ko``.

   After installing vmwgfx.ko you might want to run the following
   command to check that the new kernel module is in the expected place:

   .. code-block:: bash

      find /lib/modules -name vmwgfx.ko -exec ls -l '{}' \;

   If you see the kernel module listed in more than one place, you may
   need to move things around.

   Finally, if you update your kernel you'll probably have to rebuild
   and reinstall the vmwgfx.ko module again.

Now try to load the kernel module by issuing

.. code-block:: bash

   sudo modprobe vmwgfx

Then type

.. code-block:: bash

   dmesg

to watch the debug output. It should contain a number of lines prefixed
with "[vmwgfx]".

Then restart the Xserver (or reboot). The lines starting with
"vmwlegacy" or "VMWARE" in the file /var/log/Xorg.0.log should now have
been replaced with lines starting with "vmwgfx", indicating that the new
Xorg driver is in use.

Running OpenGL Programs
-----------------------

In a shell, run 'glxinfo' and look for the following to verify that the
driver is working:

.. code-block:: text

   OpenGL vendor string: VMware, Inc.
   OpenGL renderer string: Gallium 0.4 on SVGA3D; build: RELEASE;
   OpenGL version string: 2.1 Mesa 8.0

If you don't see this, try setting this environment variable:

.. code-block:: bash

   export LIBGL_DEBUG=verbose

then rerun glxinfo and examine the output for error messages.

If OpenGL 3.3 is not working (you only get OpenGL 2.1):

-  Make sure the VM uses hardware version 12.
-  Make sure the vmwgfx kernel module is version 2.9.0 or later.
-  Check the vmware.log file for errors.
-  Run 'dmesg \| grep vmwgfx' and look for "DX: yes".
