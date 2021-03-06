Code Repository
===============

Mesa uses `git <https://git-scm.com>`__ as its source code management
system.

The master git repository is hosted on
`freedesktop.org <https://www.freedesktop.org>`__

You may access the repository either as an `anonymous
user <#anonymous>`__ (read-only) or as a
`developer <#developer>`__\ (read/write).

You may also `browse the main Mesa git
repository <https://cgit.freedesktop.org/mesa/mesa/>`__ and the `Mesa
demos and tests git
repository <https://cgit.freedesktop.org/mesa/demos>`__.

Anonymous git Access
--------------------

To get the Mesa sources anonymously (read-only):

1. Install the git software on your computer if needed.
2. Get an initial, local copy of the repository with:

   .. code-block:: bash

      git clone git://anongit.freedesktop.org/git/mesa/mesa

3. Later, you can update your tree from the master repository with:

   .. code-block:: bash

      git pull origin

4. If you also want the Mesa demos/tests repository:

   .. code-block:: bash

      git clone git://anongit.freedesktop.org/git/mesa/demos

Developer git Access
--------------------

If you wish to become a Mesa developer with git-write privilege, please
follow this procedure:

1. Subscribe to the
   `mesa-dev <https://lists.freedesktop.org/mailman/listinfo/mesa-dev>`__
   mailing list.

2. Start contributing to the project by `submitting
   patches <submittingpatches.html>`__ to the mesa-dev list.
   Specifically,

   -  Use ``git send-mail`` to post your patches to mesa-dev.
   -  Wait for someone to review the code and give you a ``Reviewed-by``
      statement.
   -  You'll have to rely on another Mesa developer to push your initial
      patches after they've been reviewed.

3. After you've demonstrated the ability to write good code and have had
   a dozen or so patches accepted you can apply for an account.

4. Occasionally, but rarely, someone may be given a git account sooner,
   but only if they're being supervised by another Mesa developer at the
   same organization and planning to work in a limited area of the code
   or on a separate branch.

5. To apply for an account, follow `these
   directions <https://www.freedesktop.org/wiki/AccountRequests>`__.
   It's also appreciated if you briefly describe what you intend to do
   (work on a particular driver, add a new extension, etc.) in the
   bugzilla record.

Once your account is established:

1. Get an initial, local copy of the repository with:

   .. code-block:: bash

      git clone git+ssh://username@git.freedesktop.org/git/mesa/mesa

   Replace *username* with your actual login name.

2. Later, you can update your tree from the master repository with:

   .. code-block:: bash

      git pull origin

3. If you also want the Mesa demos/tests repository:

   .. code-block:: bash

      git clone git+ssh://username@git.freedesktop.org/git/mesa/demos

Windows Users
-------------

If you're `using git on
Windows <https://git.wiki.kernel.org/index.php/WindowsInstall>`__ you'll
want to enable automatic CR/LF conversion in your local copy of the
repository:

.. code-block:: bash

   git config --global core.autocrlf true

This will cause git to convert all text files to CR+LF on checkout, and
to LF on commit.

Unix users don't need to set this option.

Development Branches
--------------------

At any given time, there may be several active branches in Mesa's
repository. Generally, master contains the latest development (unstable)
code while a branch has the latest stable code.

The command ``git-branch`` will list all available branches.

Questions about branch status/activity should be posted to the
mesa3d-dev mailing list.

Developer Git Tips
------------------

1. Setting up to edit the master branch

   If you try to do a pull by just saying ``git pull`` and git complains
   that you have not specified a branch, try:

   .. code-block:: bash

      git config branch.master.remote origin
      git config branch.master.merge master

   Otherwise, you have to say ``git pull origin master`` each time you
   do a pull.

2. Small changes to master

   If you are an experienced git user working on substantial
   modifications, you are probably working on a separate branch and
   would rebase your branch prior to merging with master. But for small
   changes to the master branch itself, you also need to use the rebase
   feature in order to avoid an unnecessary and distracting branch in
   master.

   If it has been awhile since you've done the initial clone, try

   .. code-block:: bash

      git pull

   to get the latest files before you start working.

   Make your changes and use

   .. code-block:: bash

      git add <files to commit>
      git commit

   to get your changes ready to push back into the fd.o repository.

   It is possible (and likely) that someone has changed master since you
   did your last pull. Even if your changes do not conflict with their
   changes, git will make a fast-forward merge branch, branching from
   the point in time where you did your last pull and merging it to a
   point after the other changes.

   To avoid this,

   .. code-block:: bash

      git pull --rebase
      git push

   If you are familiar with CVS or similar system, this is similar to
   doing a ``cvs update`` in order to update your source tree to the
   current repository state, instead of the time you did the last
   update. (CVS doesn't work like git in this respect, but this is
   easiest way to explain it.)

   In any case, your repository now looks like you made your changes
   after all the other changes.

   If the rebase resulted in conflicts or changes that could affect the
   proper operation of your changes, you'll need to investigate those
   before doing the push.

   If you want the rebase action to be the default action, then

   .. code-block:: bash

      git config branch.master.rebase true
      git config --global branch.autosetuprebase=always

   See `Understanding Git
   Conceptually <https://www.eecs.harvard.edu/~cduan/technical/git/>`__
   for a fairly clear explanation about all of this.
