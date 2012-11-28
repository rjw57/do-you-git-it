The single dude workflow
========================

A single dude tends to commit linearly
--------------------------------------

.. graphviz:: single_dude.dot

Create a 'known good' branch
----------------------------

.. notslides::
    There may be a ``give-to-others`` branch. A 'known good' one.

.. graphviz:: single_dude2.dot
    
Note that the green commit is different to the yellow commit...

git mastery: ``git cherry-pick``
--------------------------------

.. notslides::

    Demo this

.. code-block:: console
    
    $ git cherry-pick <commit>

Merging into ``master`` when done
---------------------------------

.. graphviz:: single_dude3.dot

Merge into ``give-to-others`` when not done
-------------------------------------------

.. graphviz:: single_dude4.dot

