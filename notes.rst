Notes
=====


git
---

We'll examine how git does things in a bit more depth.

The central change which git makes which is different to SVN, CVS and RCS is that instead of having each file stored as
difference from another, git stores the files directly.

Each file which makes up a project is collected together into a single set. A node in a graph (nearly). This node
contains all of the files and a set of pointers pointing to what versions preceded it.

Initialise git repo
-------------------

The index
'''''''''

Workflows
---------


Team with central branch
''''''''''''''''''''''''

.. graphviz::

    digraph G {
        "master" [ shape=note ];
        "HEAD" [ shape=note ];

        mergea [ label="Merge feature-a into master" ];
        mergeb [ label="Merge feature-b into master" ];
        mergec [ label="Merge feature-c into master" ];

        mergeb -> mergec -> mergea -> "Previous work";

        "Finish B" -> "Start B";
        "Start A";
        "Finish C" -> "C" -> "Start C";

        mergea -> "Start A" -> "Previous work";

        mergeb -> "Finish B";
        "Start B" -> mergea;

        mergec -> "Finish C";
        "Start C" -> mergea;

        "master" -> mergeb;
        "HEAD" -> mergeb;
    }

Creating a remote git repo
--------------------------

Manually
''''''''

