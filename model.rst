The git model
=============

The commit
----------

.. digraph:: G

    node [shape=Mrecord];
    init [label="{Message|Contents|Parent(s)}"];

.. notslides::
    A commit consists of a `commit message`, `content` and zero or more `parents`. The message is intended for humans, the
    content is all the files inside that commit, their names, UNIX permissions, etc. and a parent is a pointer to another
    commit.

    This is a commit.

A repository
------------

.. code-block:: console 

    $ git add .                         # We'll see what this is later
    $ git commit -m 'First commit'

.. graphviz::

    digraph G {
        master [ shape=note ];
        HEAD [ shape=note ];

        node [shape=Mrecord];
        init [label="{First commit|Contents|null}"];

        master -> init;
        HEAD -> master;
    }

.. notslides::
    And this is a git repository with one commit.

    The note-shaped nodes are `references` or 'refs' for short. These are often called `branches` in other source
    control system. It is just a handy human-friendly label for that particular commit. Nothing more, nothing less. The
    ``HEAD`` reference is special: it points to the commit that you're currently working on. "Checking out" a branch
    simply moves the ``HEAD`` reference to that commit,

Adding a commit
---------------

.. code-block:: console 

    $ vim a.txt
    $ git commit -a -m 'Add feature A'

.. notslides::
    The ``-a`` option to ``git commit`` is a convenience option which says 'commit all the changes to all the files we
    know about.'

.. graphviz::

    digraph G {
        master [ shape=note ];
        HEAD [ shape=note ];

        node [shape=Mrecord];
        init [label="{First commit|Contents|null}"];
        c1 [label="{Add feature A|Contents|Parent}"];

        c1 -> init;
        master -> c1;
        HEAD -> master;
    }

.. notslides::
    Let's add a commit.

    In other source control systems like SVN, the 'commit' is actually the arrow; it records what is different between the
    bottom node and the top node. In git, conceptually, each commit stores the complete contents of the project at that
    point.

Adding a branch
---------------

.. code-block:: console 

    $ git checkout -b feature-b

.. graphviz::

    digraph G {
        master [ shape=note ];
        "feature-b" [ shape=note ];
        HEAD [ shape=note ];

        node [shape=Mrecord];
        init [label="{First commit|Contents|null}"];
        c1 [label="{Add feature A|Contents|Parent}"];

        c1 -> init;
        master -> c1;
        "feature-b" -> c1;
        HEAD -> "feature-b";
    }

Committing to ``feature-b``
---------------------------

.. notslides::
    Adding a commit to the ``feature-b`` branch moves the label and ``HEAD``.

.. code-block:: console 

    $ vim b.txt
    $ git add b.txt
    $ git commit -m 'Start feature B'

.. graphviz::

    digraph G {
        master [ shape=note ];
        "feature-b" [ shape=note ];
        HEAD [ shape=note ];

        node [shape=Mrecord];
        init [label="{First commit|Contents|null}"];
        c1 [label="{Add feature A|Contents|Parent}"];
        c2 [label="{Start feature B|Contents|Parent}"];

        c2 -> c1 -> init;
        master -> c1;
        "feature-b" -> c2;
        HEAD -> "feature-b";
    }

Checking out ``master``
-----------------------

.. notslides::
    Checking out ``master`` just moves the ``HEAD`` label.

.. code-block:: console 

    $ git checkout master
    $ ls                    # no b.txt; checkout updates the working copy

.. graphviz::

    digraph G {
        master [ shape=note ];
        "feature-b" [ shape=note ];
        HEAD [ shape=note ];

        node [shape=Mrecord];
        init [label="{First commit|Contents|null}"];
        c1 [label="{Add feature A|Contents|Parent}"];
        c2 [label="{Start feature B|Contents|Parent}"];

        c2 -> c1 -> init;
        master -> c1;
        "feature-b" -> c2;
        HEAD -> master;
    }

Committing to ``master``
------------------------

.. notslides::
    Let's update the master branch.

.. code-block:: console 

    $ vim a.txt
    $ git commit -a -m 'Fix bug in A'

.. graphviz::

    digraph G {
        master [ shape=note ];
        "feature-b" [ shape=note ];
        HEAD [ shape=note ];

        node [shape=Mrecord];
        init [label="{First commit|Contents|null}"];
        c1 [label="{Add feature A|Contents|Parent}"];
        c2 [label="{Start feature B|Contents|Parent}"];
        c3 [label="{Fix bug in A|Contents|Parent}"];

        c2 -> c1 -> init;
        c3 -> c1;
        master -> c3;
        "feature-b" -> c2;
        HEAD -> master;
    }

Meanwhile we also finished B
----------------------------

.. code-block:: console 

    $ git checkout feature-b
    $ vim b.txt
    $ git commit -a -m 'Finish B'; git checkout master

.. graphviz::

    digraph G {
        master [ shape=note ];
        "feature-b" [ shape=note ];
        HEAD [ shape=note ];

        node [shape=Mrecord];
        init [label="{First commit|Contents|null}"];
        c1 [label="{Add feature A|Contents|Parent}"];
        c2 [label="{Start feature B|Contents|Parent}"];
        c3 [label="{Fix bug in A|Contents|Parent}"];
        c4 [label="{Finish B|Contents|Parent}"];

        c4 -> c2 -> c1 -> init;
        c3 -> c1;
        master -> c3;
        "feature-b" -> c4;
        HEAD -> master;
    }

Merge B into ``master``
-----------------------

.. code-block:: console 

    $ git merge feature-b

.. graphviz::

    digraph G {
        master [ shape=note ];
        "feature-b" [ shape=note ];
        HEAD [ shape=note ];

        node [shape=Mrecord];
        init [label="{First commit|Contents|null}"];
        c1 [label="{Add feature A|Contents|Parent}"];
        c2 [label="{Start feature B|Contents|Parent}"];
        c3 [label="{Fix bug in A|Contents|Parent}"];
        c4 [label="{Finish B|Contents|Parent}"];
        merge [label="{Merge feature-b into master|Contents|{<p1> Parent 1|<p2> Parent 2}}"];

        c4 -> c2 -> c1 -> init;
        c3 -> c1;
        merge:p1 -> c3;
        merge:p2 -> c4;
        master -> merge;
        "feature-b" -> c4;
        HEAD -> master;
    }

Usually we simplify this diagram
--------------------------------

Leaving out the redundant 'contents' and 'parents' parts of the nodes.

.. graphviz::

    digraph G {
        master [ shape=note ];
        "feature-b" [ shape=note ];
        HEAD [ shape=note ];

        init [label="First commit"];
        c1 [label="Add feature A"];
        c2 [label="Start feature B"];
        c3 [label="Fix bug in A"];
        c4 [label="Finish B"];
        merge [label="Merge feature-b into master"];

        c4 -> c2 -> c1 -> init;
        c3 -> c1;
        merge -> c3;
        merge -> c4;
        master -> merge;
        "feature-b" -> c4;
        HEAD -> master;
    }

Merges are not symmetric
------------------------

Merging ``feature-b`` into ``master`` is not the same as ``master`` into ``feature-b``. The ``git merge`` command always
merges `into` the ``HEAD`` branch.

.. graphviz::

    digraph G {
        master [ shape=note ];
        "feature-b" [ shape=note ];
        HEAD [ shape=note ];

        init [label="First commit"];
        c1 [label="Add feature A"];
        c2 [label="Start feature B"];
        c3 [label="Fix bug in A"];
        c4 [label="Finish B"];
        merge [label="Merge master into feature-b"];

        c4 -> c2 -> c1 -> init;
        c3 -> c1;
        merge -> c4;
        merge -> c3;
        master -> c3;
        "feature-b" -> merge;
        HEAD -> "feature-b";
    }

The index
---------

The index is a node we modify which will become the next ``HEAD``. It is a temporary node.

.. graphviz::

    digraph G {
        master [ shape=note ];
        HEAD [ shape=note ];
        INDEX [ shape=note ];

        init [label="First commit"];
        c1 [label="Add feature A"];
        c2 [label="Fix bug in A"];

        c2 -> c1 -> init;
        master -> c2;
        HEAD -> master;

        c3 [label="More bug fixes", style=filled, fillcolor=yellow];
        INDEX -> c3 -> c2;
    }

git mastery: the ``git status`` command
---------------------------------------

If you're not sure what files are in the index, what are not known to git and what have not been modified, one can use
the ``git status`` command to find out.

.. note::

    The output from ``git status`` includes helpful messages about what to do to undo any changes.

Updating the index
------------------
    
'Committing' is the act of moving the labels ``master`` and
``HEAD`` to the new commit and creating a new index. 

.. notslides::
    Updating the index doesn't change ``master`` or ``HEAD``.
    The new index's contents is `exactly` the same as the "More bug
    fixes" node.

.. graphviz::

    digraph G {
        master [ shape=note ];
        HEAD [ shape=note ];
        INDEX [ shape=note ];

        init [label="First commit"];
        c1 [label="Add feature A"];
        c2 [label="Fix bug in A"];
        c3 [label="More bug fixes"];

        c3 -> c2 -> c1 -> init;
        master -> c3;
        HEAD -> master;

        INDEX -> c3;
    }

git mastery: partial add
------------------------

The ``git add`` command shuffles things into the index. The ``git add -p`` command let's us add things bit by bit.

.. code-block:: console

    $ vim a.txt b.txt
    $ git add -p
    $ git commit -m 'change 1'
    $ git add -p
    $ fir commit -m 'change 2'
    $ git add .                 # Note just 'git add' pronts out something useful

.. note::

    ``git commit -a`` adds all the changes to the index before committing.
