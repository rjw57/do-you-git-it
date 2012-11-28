What is "source control"
========================

Collaboration
-------------

.. notslides::
    Let's consider a typical workflow for a paper. I wonder if this'll sound familiar to you.

    Your paper consists of a draft of the words and a set of data. For the sake of argument we'll assume to former is in the
    shape of a Word document and the latter in the shape of a spreadsheet.  When it's all on your machine, in your head,
    it's OK.

    But you need to send the words to your supervisor or PI.

    And you need to send the data to that guy who's really good at making pretty plots.

    And you start fixing some bugs.

    Your supervisor emails you back a new document with some fixes which you start merging with your own work.

    The guy plotting the figures doesn't understand some things and so asks your supervisor for the paper draft. Your
    supervisor sends it but she sends a third version which contains some more fixes.

    Then the plotting guy emails your supervisor the data because she wants to check some things. But it's the data with the
    section not relevant to the data removed.

    And your supervisor can't reproduce your results.

    And so she emails you saying 'these words don't match these figures'. And you go 'where on Earth did these words and
    figures come from?'

    So you trace back. Asking who got what from whom. You try to reconstruct this network based on people saying 'I got this
    version of the document with the change to section 3 from here'.

    Ultimately you end up confused.

    Why does this happen?

.. digraph:: G

    subgraph cluster_you {
        label = "You";
        p1 [ label="Paper draft" ];
        d1 [ label="Data" ];
        p3 [ label="Paper draft 2" ];
        merging [ label="Paper draft 2 and a bit" ];
        WTF [ style=filled, fillcolor=pink, label="WTF is draft 3?\nWTF is this data?" ];
    }

    subgraph cluster_supervisor {
        label = "Supervisor";
        p2 [ label="Paper draft" ];
        p4 [ label="Paper draft 2" ];
        p6 [ label="Paper draft 3" ];
        d5 [ label="Data" ];
        "Conflict" [ style=filled, fillcolor=pink ];
    }

    subgraph cluster_bob {
        label = "Bob";
        d2 [ label="Data" ];
        d3 [ label="Fixed data" ];
        p5 [ label="Paper draft" ];
    }

    d1 -> d2 [ label="Email" ];
    d2 -> d3 [ label="Fixes" ];

    p1 -> p2 [ label="Email" ];

    p3 -> merging;
    p4 -> p6 [ label="Fixes" ];
    p6 -> p5 [ label="Email" ];
    p4 -> merging [ label="Email" ];

    p2 -> p4 [ label="Fixes" ];
    p4 -> p5 [ label="Email" ];

    p1 -> p3 [ label="Fix bugs" ];

    d3 -> d5 [ label="Email" ];

    d5 -> "Conflict";
    p6 -> "Conflict";

    "Conflict" -> WTF [ label="Data doesn't match draft 3" ];
    merging -> WTF;

Sole research
-------------

.. notslides::
    Now let's assume you're a sole researcher working on your own paper and don't need to pass things around. Your work
    consists of some data, some MATLAB scripts and a draft of a paper.

    You draft the paper, generate some plots and send it off to the journal.

    You carry on researching. Hacking the scripts, making some LaTeX notes, tweaking the data.

    It is accepted! But with corrections. You need to regenerate some plots. But what script did you use? What data? Do you
    even have the original paper version?

    You were clever though: you copied everything after submitting the paper to a directory with '-for-submission' appended
    to it. But then you remember: that version had a problem you corrected since submitting the paper. Where was that
    correction again?

    Ultimately you end up confused.

    Why does this happen?

    In the first example confusion happened because no-one knew the route through the network of emails that any one
    document took to get to them and hence had no idea whether anyone else had that exact same document. It also happened
    because the data was separated from the document which was supposed to describe it.

    In the second example confusion happened because there was no sense of `history` to the project. You had a coarse
    history divided into Before Corrections and After Dicking around but no record of what that dicking was or why you did
    it.

    In this talk I'm going to show you what `automated` solutions exist to the problem above. Obviously automated solutions
    are not the only solutions. Having a clear, documented workflow for paper editing with a designated 'contact person'
    would have helped in the first place. Keeping better notes about what you did would have helped in the second.

    Non-automated workflows work perfectly in the case of perfect people. Unfortunately people are rarely perfect. Computers
    aren't perfect either but they do excel and being boring, pedantic and officious. Exactly what you need if you're trying
    to record the progress of research.

.. digraph:: G

    subgraph cluster_you {
        label = "You";

        paper1 [ label="Paper v1" ];
        paper1b [ label="Paper v1 (fixed)" ];
        paper2 [ label="Paper v2" ];

        data1 [ label="Data v1" ];
        data2 [ label="Data v2" ];

        scripts1 [ label="Scripts v1" ]; 
        scripts2 [ label="Scripts v2" ]; 
        scripts3 [ label="Scripts v3" ]; 

        WTF [ 
            style=filled, fillcolor=pink,
            label="What version was this?\nWhat data did I use?\nWhat scripts did I use?"
        ];

        paper1 -> paper2;
        paper1 -> paper1b;
        data1 -> data2;
        scripts1 -> scripts2 -> scripts3;
        paper2 -> WTF [ style=invis ];
    }

    subgraph cluster_journal {
        label = "Journal";
        papersub [ label="Paper submission" ];
    }

    paper1 -> papersub [ label="Submit" ];
    papersub -> WTF [ label="Correct plots" ];

Source control
--------------

A technical solution designed to keep track who who gave what to whom and when, what it is and how it differs from what
you have.

This for all :math:`t`.

It does `not` remove the requirement to manage your project. It is a `tool` not a `panacea`.

**Corollary:** use the `right` tool for the job.

Desiderata
----------

* Concurrent
* Many files make one project
* Many versions are 'current'
* Uniquely name a version or a file
* Stably name a version or a file
* What is the history of this version?
* Technically useful
 * Merging should be easy
 * Can send you my version (connected)
 * You can get it from just the name (content addressed)

.. notslides::

    So, let's try to learn from these examples and outline a system that would be useful to us:

    * More than one person can work on a project at once. [Concurrent]

    * A project is more than a single file. A research project can consist of multiple files: paper drafts, datasets, MATLAB
      scripts, notes, etc. We want to track these as a single related set. [Many files]

    * More then one version of a project exists at once. Your PI might have a version, your plotting guy, the journal you're
      submitting to. In addition you want to keep multiple versions around: at the least a 'known good' and 'what I'm
      working on' version. [Many versions]

    * Can name a particular version. Often overlooked, I want to be able to uniquely refer to a particular version in an
      efficient way. [Names]

    * Is the name `I` call a file, the same as the name someone else calls it? [Stable names]

    * Can reconstruct the network. Given one version, I'd like to be able to reconstruct the network. Ideally I'd like to
      plot it. [History]

    In addition there are some things which might be convenient from a technical perspective.

    * Make it easy to merge projects. When you get the changes back from the PI and some new data from your technicians, can
      the computer help with the merging? [Merging]

    * Make it easy to send the versions around. [Connected]

    * Given a document name, make it possible to get every version of every document which lead to it. This makes backups
      trivial: simply send the name somewhere. [Content addressed]
