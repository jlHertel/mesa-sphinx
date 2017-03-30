Platforms and Drivers
=====================

Mesa is primarily developed and used on Linux systems. But there's also
support for Windows, other flavors of Unix and other systems such as
Haiku. We're actively developing and maintaining several hardware and
software drivers.

The primary API is OpenGL but there's also support for OpenGL ES 1, ES2
and ES 3, OpenVG, OpenCL, VDPAU, XvMC and the EGL interface.

Hardware drivers include:

-  Intel i965, i945, i915. See `Intel's website <https://01.org/linuxgraphics>`__
-  AMD Radeon series. See `RadeonFeature <https://www.x.org/wiki/RadeonFeature>`__
-  NVIDIA GPUs. See `Nouveau Wiki <https://nouveau.freedesktop.org>`__
-  `VMware virtual GPU <https://www.x.org/wiki/vmware>`__

Software drivers include:

-  llvmpipe - uses LLVM for x86 JIT code generation and is multi-threaded
-  softpipe - a reference Gallium driver
-  swrast - the legacy/original Mesa software rasterizer

Additional driver information:

-  `DRI hardware drivers <https://dri.freedesktop.org/>`__ for the X Window System
-  `Xlib / swrast driver <xlibdriver.html>`__ for the X Window System and Unix-like operating systems
-  `Microsoft Windows <README.WIN32>`__
-  `VMware <vmware-guest.html>`__ guest OS driver

Deprecated Systems and Drivers
------------------------------

In the past there were other drivers for older GPUs and operating
systems. These have been removed from the Mesa source tree and
distribution. If anyone's interested though, the code can be found in
the git repo. The list includes:

-  3dfx/glide
-  Matrox
-  ATI R128
-  Savage
-  VIA Unichrome
-  SIS
-  3Dlabs gamma
-  DOS
-  fbdev
-  DEC/VMS

.. toctree::
   :maxdepth: 1
   :hidden:
   
   xlibdriver