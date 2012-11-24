Notes
=====

Collaboration
-------------

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

Sole research
-------------

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

Desiderata
----------

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

History
-------

How old do you think this problem of 'what version of the file am I talking about' it? Well email as we know it is based
on the Simple Mail Transfer Protocol which as first documented in 1982. This was based on earlier protocols such as the
Mail Box Protocol (c. 1971, disputed), Mail Protocol (c. 1973) and FTP Mail (c. 1973).

So networked email first became available on networks around the early seventies. But people didn't use it. They had
little need within an institution for the simple reason that in any one institution there was probably only one
computer.

Instead they used a 'time sharing' system in which multiple keyboards and, in modern parlance, monitors were connected
to a single machine giving each user the illusion of being the sole user of the machine. In the modern world web servers
do this: a website gives you the impression you have its undivided attention whereas in actual fact the computer running
the website is constantly serving requests to other people with your requests 'interleaved' in.

In those days systems worked a bit like a modern SOHO. There was a single 'shared' area which people put documents to be
worked on collaboratively. And like today, this failed horribly [slide idea: 'cannot be opened as someone else has
opened it' error?].

The modern concept of a file and a shared 'drive' was invented in 1969 at Bell Labs as part of the UNIX system; a system
to allow for the collaborative typing of patent applications by inventors and typists note. The first program designed
to deal with the multiple version hell was written in 1972, at Bell Labs, for UNIX.

They lasted 3 years before deciding that they needed to invent a whole new class of program.

SCCS and RCS
------------

This was called the 'SCCS' (Source code control system) and, as the name suggested, was a control system for 'source
code'. Source code are the files which a human edits which go to make up some final product. For a computer program, the
source code tends to be text files written in special languages such as C, C++, FORTRAN, Python or SNOBOL. For a paper,
they can be Word documents and figures. (Or the data and spreadsheets which make the figures.) For PhD source code tends
to be MATLAB scripts, prayer and coffee.

SCCS was largely replaced in 1982 by RCS (Revision control system) which was mostly written as a free version of SCCS.
Yes, Open Source Software enthusiasts existed back then. It also had the benefit of 10 years of experience with SCCS to
draw on but operated in a very similar manner.

Next to a file, ``foo.txt``, say, a separate file, ``foo.txt.v`` was maintained which contained the differences from
the current file right back in time.

The workflow was thus: checkout the current file version, modify the file until it's at a good spot, and 'checkin' the
file with a descriptive message. The RCS utility would then record the difference between your file and the top of the
deltas and store the new delta with the message.

Let's mark RCS with our criteria:

* Concurrent, No [The 'checkout'/'checkin' is protected by a lock.]
* Many files, No
* Many versions, No
* Names, Yes
* Stable names, No
* History, Yes
* Merging, No
* Connected, No
* Content addressed, No

A good start but not there yet. RCS served people well for many years. In fact Microsoft Sharepoint essentially
implements the RCS model and people seem to find it useful.

CVS
---

The greatest annoyance of RCS was the lack of concurrency. For a single researcher this isn't too much of a problem but
if that researcher were to tell their PI to check the latest version of the paper out of RCS and edit it, the researcher
could not themselves check it out until their PI had 'checked in' the file again.

And so was born, in 1990, CVS the Concurrent Versioning System. This worked just like RCS: each file was separately
versioned
