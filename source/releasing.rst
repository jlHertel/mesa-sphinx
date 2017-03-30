Releasing process
=================

Overview
--------

This document uses the convention X.Y.Z for the release number with X.Y
being the stable branch name.

Mesa provides feature and bugfix releases. Former use zero as patch
version (Z), while the latter have a non-zero one.

For example:

.. code-block:: text

    Mesa 10.1.0 - 10.1 branch, feature
    Mesa 10.1.4 - 10.1 branch, bugfix
    Mesa 12.0.0 - 12.0 branch, feature
    Mesa 12.0.2 - 12.0 branch, bugfix

Release schedule
----------------

Releases should happen on Fridays. Delays can occur although those
should be keep to a minimum.

Feature releases
~~~~~~~~~~~~~~~~

-  Available approximately every three months.
-  Initial timeplan available 2-4 weeks before the planned branchpoint
   (rc1) on the mesa-announce@ mailing list.
-  A `pre-release <#prerelease>`__ announcement should be available
   approximately 24 hours before the final (non-rc) release.

Stable releases
~~~~~~~~~~~~~~~

-  Normally available once every two weeks.
-  Only the latest branch has releases. See note below.
-  A `pre-release <#prerelease>`__ announcement should be available
   approximately 48 hours before the actual release.

Note: There is one or two releases overlap when changing branches. For
example:

The final release from the 12.0 series Mesa 12.0.5 will be out around
the same time (or shortly after) 13.0.1 is out.

Cherry-picking and testing
--------------------------

Commits nominated for the active branch are picked as based on the
`criteria <submittingpatches.html#criteria>`__ as described in the same
section.

Maintainer is responsible for testing in various possible permutations
of the autoconf and scons build.

Cherry-picking and build/check testing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Done continuously up-to the `pre-release <#prerelease>`__ announcement.

As an exception, patches can be applied up-to the last ~1h before the
actual release. This is made **only** with explicit permission/request,
and the patch **must** be very well contained. Thus it cannot affect
more than one driver/subsystem.

Currently Ilia Mirkin and AMD devs have requested "permanent" exception.

-  make distcheck, scons and scons check must pass
-  Testing with different version of system components - LLVM and others
   is also performed where possible.

Achieved by combination of local ad-hoc scripts and AppVeyor plus
Travis-CI, the latter as part of their Github integration.

Regression/functionality testing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Less often (once or twice), shortly before the pre-release announcement.
Ensure that testing is redone if Intel devs have requested an exception,
as per above.

-  *no regressions should be observed for Piglit/dEQP/CTS/Vulkan on
   Intel platforms*
-  *no regressions should be observed for Piglit using the swrast,
   softpipe and llvmpipe drivers*

Currently testing is performed courtesy of the Intel OTC team and their
Jenkins CI setup. Check with the Intel team over IRC how to get things
setup.

Making a branchpoint
--------------------

A branchpoint is made such that new development can continue in parallel
to stabilisation and bugfixing.

Note: Before doing a branch ensure that basic build and ``make check``
testing is done and there are little to-no issues.

Ideally all of those should be tackled already.

Check if the version number is going to remain as, alternatively
``git mv docs/relnotes/{current,new}.html`` as appropriate.

To setup the branchpoint:

.. code-block:: bash

   git checkout master # make sure we're in master first
   git tag -s X.Y-branchpoint -m "Mesa X.Y branchpoint"
   git checkout -b X.Y
   git checkout master
   $EDITOR VERSION # bump the version number
   git commit -as
   cp docs/relnotes/{X.Y,X.Y+1}.html # copy/create relnotes template
   git commit -as
   git push origin X.Y-branchpoint X.Y

Now go to
`Bugzilla <https://bugs.freedesktop.org/editversions.cgi?action=add&product=Mesa>`__
and add the new Mesa version X.Y.

Check that there are no distribution breaking changes and revert them if
needed. For example: files being overwritten on install, etc. Happens
extremely rarely - we had only one case so far (see commit
2ced8eb136528914e1bf4e000dea06a9d53c7e04).

Proceed to `release <#release>`__ -rc1.

Pre-release announcement
------------------------

It comes shortly after outstanding patches in the respective branch are
pushed. Developers can check, in brief, what's the status of their
patches. They, alongside very early testers, are strongly encouraged to
test the branch and report any regressions.

It is followed by a brief period (normally 24 or 48 hours) before the
actual release is made.

Terminology used
~~~~~~~~~~~~~~~~

Nominated
    Patch that is nominated but yet to to merged in the patch
    queue/branch.
Queued
    Patch is in the queue/branch and will feature in the next release.
    Barring reported regressions or objections from developers.
Rejected
    Patch does not fit the
    `criteria <submittingpatches.html#criteria>`__ and is followed by a
    brief information. The release maintainer is human so if you believe
    you've spotted a mistake do let them know.

Format/template
~~~~~~~~~~~~~~~

.. code-block:: bash

    Subject: [ANNOUNCE] Mesa X.Y.Z release candidate
    To: mesa-announce@...
    Cc: mesa-dev@...

    Hello list,

    The candidate for the Mesa X.Y.Z is now available. Currently we have:
     - NUMBER queued
     - NUMBER nominated (outstanding)
     - and NUMBER rejected patches

    BRIEF SUMMARY OF CHANGES

    Take a look at section "Mesa stable queue" for more information.


    Testing reports/general approval
    --------------------------------
    Any testing reports (or general approval of the state of the branch) will be
    greatly appreciated.

    The plan is to have X.Y.Z this DAY (DATE), around or shortly after TIME.

    If you have any questions or suggestions - be that about the current patch
    queue or otherwise, please go ahead.


    Trivial merge conflicts
    -----------------------
    List of commits where manual intervention was required.
    Keep the authors in the CC list.

    commit SHA
    Author: AUTHOR

        COMMIT SUMMARY

        CHERRY PICKED FROM


    For example:

    commit 990f395e007c3204639daa34efc3049f350ee819
    Author: Emil Velikov &lt;emil.velikov@collabora.com&gt;

        anv: automake: cleanup the generated json file during make clean

        (cherry picked from commit 8df581520a823564be0ab5af7dbb7d501b1c9670)


    Cheers,
    Emil


    Mesa stable queue
    -----------------

    Nominated (NUMBER)
    ==================

    AUTHOR (NUMBER):
          SHA     COMMIT SUMMARY

    For example:

    Dave Airlie (1):
          2de85eb radv: fix texturesamples to handle single sample case


    Queued (NUMBER)
    ===============

    AUTHOR (NUMBER):
          COMMIT SUMMARY


    Rejected (NUMBER)
    =================

    Rejected (11)
    =============

    AUTHOR (NUMBER):
          SHA     COMMIT SUMMARY

    Reason: ...

Making a new release
--------------------

These are the instructions for making a new Mesa release.

Get latest source files
~~~~~~~~~~~~~~~~~~~~~~~

Ensure the latest code is available - both in your local master and the
relevant branch.

Perform basic testing
~~~~~~~~~~~~~~~~~~~~~

Most of the testing should already be done during the
`cherry-pick <#pickntest>`__ and `pre-announce <#prerelease>`__ stages.

So we do a quick 'touch test'

-  make distcheck (you can omit this if you're not using --dist below)
-  scons (from release tarball)
-  the produced binaries work

Here is one solution that I've been using.

.. code-block:: bash

    git clean -fXd; git clean -nxd
    read # quick cross check any outstanding files
    export __version=`cat VERSION`
    export __mesa_root=../
    export __build_root=./foo
    chmod 755 -fR $__build_root; rm -rf $__build_root
    mkdir -p $__build_root &amp;&amp; cd $__build_root

    $__mesa_root/autogen.sh --enable-llvm-shared-libs &amp;&amp; make -j2 distcheck

    # Build check the tarballs (scons, linux)
    tar -xaf mesa-$__version.tar.xz &amp;&amp; cd mesa-$__version
    scons
    cd .. &amp;&amp; rm -rf mesa-$__version

    # Build check the tarballs (scons, windows/mingw)
    tar -xaf mesa-$__version.tar.xz &amp;&amp; cd mesa-$__version
    scons platform=windows toolchain=crossmingw
    cd .. &amp;&amp; rm -rf mesa-$__version

    # Test the automake binaries
    tar -xaf mesa-$__version.tar.xz &amp;&amp; cd mesa-$__version
    ./configure \
        --with-dri-drivers=i965,swrast \
        --with-gallium-drivers=swrast \
        --with-vulkan-drivers=intel \
        --enable-llvm-shared-libs \
        --enable-llvm \
        --enable-glx-tls \
        --enable-gbm \
        --enable-egl \
        --with-egl-platforms=x11,drm,wayland
    make -j2 &amp;&amp; DESTDIR=`pwd`/test make -j6 install
    __glxinfo_cmd='glxinfo 2>&amp;1 | egrep -o "Mesa.*|Gallium.*|.*dri\.so"'
    __glxgears_cmd='glxgears 2>&amp;1 | grep -v "configuration file"'
    __es2info_cmd='es2_info 2>&amp;1 | egrep "GL_VERSION|GL_RENDERER|.*dri\.so"'
    __es2gears_cmd='es2gears_x11 2>&amp;1 | grep -v "configuration file"'
    export LD_LIBRARY_PATH=`pwd`/test/usr/local/lib/
    export LIBGL_DRIVERS_PATH=`pwd`/test/usr/local/lib/dri/
    export LIBGL_DEBUG=verbose
    eval $__glxinfo_cmd
    eval $__glxgears_cmd
    eval $__es2info_cmd
    eval $__es2gears_cmd
    export LIBGL_ALWAYS_SOFTWARE=1
    eval $__glxinfo_cmd
    eval $__glxgears_cmd
    eval $__es2info_cmd
    eval $__es2gears_cmd
    export LIBGL_ALWAYS_SOFTWARE=1
    export GALLIUM_DRIVER=softpipe
    eval $__glxinfo_cmd
    eval $__glxgears_cmd
    eval $__es2info_cmd
    eval $__es2gears_cmd
    # Smoke test DOTA2
    unset LD_LIBRARY_PATH
    unset LIBGL_DRIVERS_PATH
    unset LIBGL_DEBUG
    unset LIBGL_ALWAYS_SOFTWARE
    export VK_ICD_FILENAMES=`pwd`/src/intel/vulkan/dev_icd.json
    steam steam://rungameid/570  -vconsole -vulkan

Update version in file VERSION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Increment the version contained in the file VERSION at Mesa's top-level,
then commit this change.

Create release notes for the new release
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new file docs/relnotes/X.Y.Z.html, (follow the style of the
previous release notes). Note that the sha256sums section of the release
notes should be empty (TBD) at this point.

Two scripts are available to help generate portions of the release
notes:

.. code-block:: bash

   ./bin/bugzilla_mesa.sh
   ./bin/shortlog_mesa.sh

The first script identifies commits that reference bugzilla bugs and
obtains the descriptions of those bugs from bugzilla. The second script
generates a log of all commits. In both cases, HTML-formatted lists are
printed to stdout to be included in the release notes.

Commit these changes and push the branch.

.. code-block:: bash

    git push origin HEAD

Use the release.sh script from xorg util-macros
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ensure that the mesa git tree is clean via ``git clean -fXd`` and start
the release process.

.. code-block:: bash

   ../relative/path/to/release.sh . # append --dist if you've already done distcheck above

Pay close attention to the prompts as you might be required to enter
your GPG and SSH passphrase(s) to sign and upload the files,
respectively.

Add the sha256sums to the release notes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Edit docs/relnotes/X.Y.Z.html to add the sha256sums as available in the
mesa-X.Y.Z.announce template. Commit this change.

Back on mesa master, add the new release notes into the tree
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Something like the following steps will do the trick:

.. code-block:: bash

   git cherry-pick -x X.Y~1
   git cherry-pick -x X.Y

Also, edit docs/relnotes.html to add a link to the new release notes,
and edit docs/index.html to add a news entry. Then commit and push:

.. code-block:: bash

   git commit -as -m "docs: add news item and link release notes for X.Y.Z"
   git push origin master X.Y

Announce the release
--------------------

Use the generated template during the releasing process.

Update the mesa3d.org website
-----------------------------

As the hosting was moved to freedesktop, git hooks are deployed to
update the website. Manually check that it is updated 5-10 minutes after
the final ``git push``

Update Bugzilla
---------------

Parse through the bugreports as listed in the docs/relnotes/X.Y.Z.html
document.

If there's outstanding action, close the bug referencing the commit ID
which addresses the bug and mention the Mesa version that has the fix.

Note: the above is not applicable to all the reports, so use common
sense.
