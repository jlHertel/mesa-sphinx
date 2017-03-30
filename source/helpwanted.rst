Help Wanted / To-Do List
========================

We can always use more help with the Mesa project. Here are some
specific ideas and areas where help would be appreciated:

1. **Driver patching and testing.** Patches are often posted to the
   `mesa-dev mailing
   list <https://lists.freedesktop.org/mailman/listinfo/mesa-dev>`__,
   but aren't immediately checked into git because not enough people are
   testing them. Just applying patches, testing and reporting back is
   helpful.

2. **Driver debugging.** There are plenty of open bugs in the `bug
   database <https://bugs.freedesktop.org/describecomponents.cgi?product=Mesa>`__.

3. **Remove aliasing warnings.** Enable gcc -Wstrict-aliasing=2
   -fstrict-aliasing and track down aliasing issues in the code.

4. **Windows driver building, testing and maintenance.** Fixing MSVC
   builds.

5. **Contribute more tests to
   `Piglit <https://piglit.freedesktop.org/>`__.**

6. **Automatic testing.** It would be great if someone would set up an
   automated system for grabbing the latest Mesa code and run tests
   (such as piglit) then report issues to the mailing list.

You can find some further To-do lists here:

Common To-Do lists
------------------

-  `features.txt <https://cgit.freedesktop.org/mesa/mesa/tree/docs/features.txt>`__
   - Status of OpenGL 3.x / 4.x features in Mesa.


-  `MissingFunctionality <https://dri.freedesktop.org/wiki/MissingFunctionality>`__
   - Detailed information about missing OpenGL features.

Driver specific To-Do lists
---------------------------

-  `LLVMpipe <https://cgit.freedesktop.org/mesa/mesa/tree/src/gallium/docs/llvm-todo.txt>`__ - Software driver using LLVM for runtime code generation.

-  `radeonsi <https://dri.freedesktop.org/wiki/RadeonsiToDo>`__ - Driver for AMD Southern Island.

-  `r600g <https://dri.freedesktop.org/wiki/R600ToDo>`__ - Driver for ATI/AMD R600 - Northern Island.

-  `r300g <https://dri.freedesktop.org/wiki/R300ToDo>`__ - Driver for ATI R300 - R500.

-  `i915g <https://cgit.freedesktop.org/mesa/mesa/tree/src/gallium/drivers/i915/TODO>`__ - Driver for Intel i915/i945.


If you want to do something new in Mesa, first join the Mesa developer's
mailing list. Then post a message to propose what you want to do, just
to make sure there's no issues.

Anyone is welcome to contribute code to the Mesa project. By doing so,
it's assumed that you agree to the code's licensing terms.

Finally:

1. Try to write high-quality code that follows the existing style.
2. Use uniform indentation, write comments, use meaningful identifiers,
   etc.
3. Test your code thoroughly. Include test programs if appropriate.
