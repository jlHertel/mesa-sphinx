Performance Tips
================

Performance tips for software rendering:

1.  Turn off smooth shading when you don't need it (glShadeModel)
2.  Turn off depth buffering when you don't need it.
3.  Turn off dithering when not needed.
4.  Use double buffering as it's often faster than single buffering
5.  Compile in the X Shared Memory extension option if it's supported on
    your system by adding -DSHM to CFLAGS and -lXext to XLIBS for your
    system in the Make-config file.
6.  Recompile Mesa with more optimization if possible.
7.  Try to maximize the amount of drawing done between glBegin/glEnd
    pairs.
8.  Use the MESA\_BACK\_BUFFER variable to find best performance in
    double buffered mode. (X users only)
9.  Optimized polygon rasterizers are employed when:

    -  rendering into back buffer which is an XImage
    -  RGB mode, not grayscale, not monochrome
    -  depth buffering is GL\_LESS, or disabled
    -  flat or smooth shading
    -  dithered or non-dithered
    -  no other rasterization operations enabled (blending, stencil,
       etc)

10. Optimized line drawing is employed when:

    -  rendering into back buffer which is an XImage
    -  RGB mode, not grayscale, not monochrome
    -  depth buffering is GL\_LESS or disabled
    -  flat shading
    -  dithered or non-dithered
    -  no other rasterization operations enabled (blending, stencil,
       etc)

11. Textured polygons are fastest when:

    -  using a 3-component (RGB), 2-D texture
    -  minification and magnification filters are GL\_NEAREST
    -  texture coordinate wrap modes for S and T are GL\_REPEAT
    -  GL\_DECAL environment mode
    -  glHint( GL\_PERSPECTIVE\_CORRECTION\_HINT, GL\_FASTEST )
    -  depth buffering is GL\_LESS or disabled

12. Lighting is fastest when:

    -  Two-sided lighting is disabled
    -  GL\_LIGHT\_MODEL\_LOCAL\_VIEWER is false
    -  GL\_COLOR\_MATERIAL is disabled
    -  No spot lights are used (all GL\_SPOT\_CUTOFFs are 180.0)
    -  No local lights are used (all position W's are 0.0)
    -  All material and light coefficients are >= zero

13. XFree86 users: if you want to use 24-bit color try starting your X
    server in 32-bit per pixel mode for better performance. That is,
    start your X server with ``startx -- -bpp 32`` instead of
    ``startx -- -bpp 24``
14. Try disabling dithering with the MESA\_NO\_DITHER environment
    variable. If this env var is defined Mesa will disable dithering and
    the command glEnable(GL\_DITHER) will be ignored.
