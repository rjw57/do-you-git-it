The many dude workflow
======================

Branch early, merge often
-------------------------

.. notslides::

    This is what the true development history looks like. Feature A took many goes to get right before it was ready.

.. digraph:: G

    subgraph cluster_a {
        label="Alice";
        feature_a [ shape=note ];
        feature_a -> "Ready for merging A" -> "Hack A" -> "Fix silly bug" -> "Start A";
    }

    subgraph cluster_b {
        label="Bob";
        feature_b [ shape=note ];
        feature_b -> "Ready for merging B" -> "Start B";
    }

    subgraph cluster_c {
        label="Eve";
        feature_c [ shape=note ];
        feature_c -> "Implement C";
    }

    subgraph cluster_server {
        label="'trunk' of development";
        HEAD [ shape=note ];
        master [ shape=note ];
        init [ label="Initial commit" ];
        merge_a [ label="Merge feature_a" ];
        merge_b [ label="Merge feature_b" ];
        merge_c [ label="Merge feature_c" ];
    }

    merge_c -> merge_b -> merge_a -> init;

    "Start A" -> init;
    merge_a -> "Ready for merging A";

    "Start B" -> merge_a;
    merge_b -> "Ready for merging B";

    "Implement C" -> merge_a;
    merge_c -> "Implement C"

    master -> merge_c
    HEAD -> master

git mastery: ``git rebase -i``
------------------------------

.. notslides::
    This is what we want to record.

.. digraph:: G

    subgraph cluster_a {
        label="Alice";
        feature_a [ shape=note ];
        feature_a -> "Implement A";
    }

    subgraph cluster_b {
        label="Bob";
        feature_b [ shape=note ];
        feature_b -> "B sub-feature 2" -> "B sub-feature 1";
    }

    subgraph cluster_c {
        label="Eve";
        feature_c [ shape=note ];
        feature_c -> "Implement C";
    }

    subgraph cluster_server {
        label="'trunk' of development";
        HEAD [ shape=note ];
        master [ shape=note ];
        init [ label="Initial commit" ];
        merge_a [ label="Merge feature_a" ];
        merge_b [ label="Merge feature_b" ];
        merge_c [ label="Merge feature_c" ];
    }

    merge_c -> merge_b -> merge_a -> init;

    "Implement A" -> init;
    merge_a -> "Implement A";

    "B sub-feature 1" -> merge_a;
    merge_b -> "B sub-feature 2";

    "Implement C" -> merge_a;
    merge_c -> "Implement C"

    master -> merge_c
    HEAD -> master
