Do You Git It?
==============

Dr Rich Wareham, ``rjw57@cam.ac.uk``, http://gplus.to/richwareham

Slides & Notes: http://richwareham.com/~rjw57/dygi/

Source: https://github.com/rjw57/do-you-git-it

Introduction
------------

* :download:`Is this for hackers? <videos/hackers.mp4>`
    * https://www.youtube.com/watch?v=bl_1OybdteY
* What is source control?
* Why should I care?
* :download:`An introduction to SVN <videos/svn-intro.mp4>`
    * https://www.youtube.com/watch?v=fVEoWkdnZ40

Source control
--------------

.. notslides::

    The introduction video said that SVN lets you store a copy and revert other's changes. That's not source control,
    it's backups and a management problem.

* It is *not* about being able to undo changes, although it certainly helps that
* It is *not* required to make something Open Source

* :doc:`why`
* :doc:`history`

Choosing source control
-----------------------

SVN *can* be the right choice.

.. slides::
    * SVN - source *archival*
    * git - source *control*
        * *Mercurial* (a.k.a. ``hg`` is also good)

.. notslides::

    If all you want is backup, there is one thrust of development and people are not working on big new features, just
    maintaining old code, use SVN. It is great for source archival

    If you want to make your code a living breathing entity which is being developed by you or many people, use git.

Git: where and how?
-------------------

* http://git-scm.com/documentation
* https://help.github.com/
* `Getting started with git for the Windows developer
  <http://typecastexception.com/post/2012/09/01/Getting-Started-with-Git-for-the-Windows-Developer-(Part-I).aspx>`_

Git: is it difficult?
---------------------

* :download:`Some people can't follow it <videos/downfall-git.mp4>`
    * https://www.youtube.com/watch?v=CDeG4S-mJts
* :download:`Aren't git users "holier than thou"? <videos/linus-tech-talk-edit.webm>`
    * https://www.youtube.com/watch?v=4XpnKHJAok8

Git: the model
--------------

* :doc:`model`

Git in one slide
----------------

Initially:

.. code-block:: console

    $ git clone <url>       # Get something on your machine

Repeatedly:

.. code-block:: console

    $ git add <filename>    # Add a newly created file (when needed)
    $ git commit -a         # Commit any changes since the last commit

Regularly:

.. code-block:: console

    $ git push              # Push your changes elsewhere
    $ git pull              # Get elsewhere's changes on your machine

Git in two slides
-----------------

Initially:

.. code-block:: console

    $ git pull                              # Get any changes
    $ git checkout -b feature-branch        # Start a new feature

Repeatedly:

.. code-block:: console

    $ # ... hack, hack, hack ...
    $ git commit -a -m 'Commit message'

Finally:

.. code-block:: console

    $ git checkout master                   # Back on 'master'
    $ git pull                              # Get any changes
    $ get merge feature-branch              # Merge *your* changes
    $ git push                              # Send them back

Git: distributing work
----------------------

* :doc:`server`

* Workflows
    * :doc:`single_dude`
    * :doc:`many_dudes`

The 'zen' of git
----------------

* Pushing and pulling are uncontroversial things
* Do all the work on *your* machine in a *short-lived* branch
* Branch early, merge often

Any other things?
-----------------

* To the audience: anything you want to know about?
