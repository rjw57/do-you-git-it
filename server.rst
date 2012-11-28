Working remotely
================

.. notslides::
    We've seen how to use git as a single user. How do we share out code with people?

    Gii is a database and just like your contacts you can sync the database around

Rolling your own server (SVN)
-----------------------------

* Install apache webserver
* Configure apache to serve files from your SVN root
* Run ``svnadmin create``
* Configure SVN permissions
* Decide on a repository layout

Rolling your own server (git)
-----------------------------

.. code-block:: console

    $ ssh richwareham.com git init --bare ~/git/my-repo.git

.. notslides::

    We'll push what we've been working on to the remote repo, delete the tree and check it out.

    The remainder will be checking out yt, making a modification locally, pushing it to our private repo.

Remotes
-------

.. notslides::

    A git 'remote' is simply the address of a database to sync with. 'Refs' on the remote server will be called
    ``remote_name/ref_name`` in your database,

.. code-block:: console

    $ git init yt; cd yt
    $ git remote add origin git@github.com:rjw57/yt.git
    $ git fetch
    $ git merge origin/master

Cloning
-------

.. notslides::

    The above is equivalent to

.. code-block:: console

    $ git clone git@github.com:rjw57/yt.git

Pushing and pulling
-------------------

**Push**

.. code-block:: console

    $ git push remote_name ref_name

Make something over there like ``HEAD``.

**Pull**

.. code-block:: console

    $ git pull remote_name ref_name

Make ``HEAD`` like something over there fetching first.

Aside: fast-forwarding
----------------------

.. digraph:: G

    HEAD [ shape=note ];
    master [ shape=note ];
    feature [ shape=note ];

    D -> C -> B -> A;

    HEAD -> master;
    feature -> D;
    master -> B;

Explicit merge
--------------

.. notslides::
    This is the explicit merging we're used to.

.. digraph:: G

    HEAD [ shape=note ];
    master [ shape=note ];
    feature [ shape=note ];

    D -> C -> B -> A;

    merge [ label="merge feature into master" ];
    merge -> B;
    merge -> D;

    HEAD -> master;
    feature -> D;
    master -> merge;

"Fast forward" merge
--------------------

.. notslides::

    If we don't care about recording 'explicit' merges, we can 'fast forward'. This just moves the labels.

.. digraph:: G

    HEAD [ shape=note ];
    master [ shape=note ];
    feature [ shape=note ];

    D -> C -> B -> A;

    HEAD -> master;
    feature -> D;
    master -> D;

Demo
----

* Signing up to ``github``
* Forking ``yt``
* Sending pull request

.. notslides::

    Create new remote for github for our yt program. Push to it. Create pull request.
