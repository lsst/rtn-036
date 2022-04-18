:tocdepth: 1

.. sectnum::

.. note::

   **This technote is a work-in-progress.**

Abstract
========

The Data Facilities have a need for identical distributions of software for executing the Data Release Production.
In addition, the USDF has a need for distributing development versions of software across its compute resources.
This short note details the various environments and how we expect to distribute software within them.


Data Release Processing
=======================

DRP will run with released software.
Even if hot-fix patches are needed, they're on a (day/week) timescale where some testing can be done and containers prepared.
We can either use Apptainer containers distributed via CVMFS or stack installations directly in CVMFS, depending on site preference.

Rubin Science Platform
======================

RSP runs by necessity from containers (Docker).
These containers are typically built on top of stack release containers.

OCPS/UWS
========

While the OCPS and its back-end UWS service are intended to run at the Summit and Base, there is the potential to execute UWS scripts at the USDF.

Note that the UWS server container and OCPS service container are distinct from the worker containers that execute pipeline jobs.
Today the latter are stack release containers (most recent daily).
But for faster startup time and more software flexibility, the back-end worker containers could be minimal OS-only containers or conda-only containers that mount the stack from a shared location.

Developer Interactive
=====================

Developers sometimes use a shared-stack installation managed by custom software.
Most of the time, new weeklies are installed automatically by a cron job.
There is a request to add dailies to this, but a means of purging old ones must be developed before that is feasible.
The shared stack requires manual updates every time a new, incompatible rubin-env is released.
CVMFS could potentially be used to distribute the shared stack.
The difference between the shared stack and CVMFS is that the former uses a single eups stack per rubin-env, allowing easy switching between different versions of a package.
The latter uses a different rubin-env installation and eups stack per release.

Developers also use "lsstsw" installations in their own personal spaces.

Other options are personal "lsstinstall" installations and "stackvana" conda installations.

On top of all of these, developers may have their own git clones of stack packages with committed and uncommitted changes.

CVMFS is not appropriate for any of these latter high-change-frequency uses.

Developer Batch
===============

Ideally all of the interactive software sources are also available to batch nodes.

If not, some fast packaging mechanism to turn the current set of setup eups packages into a viable distribution for use on the batch nodes needs to be created.
To enable rapid development cycles, this cannot take longer than a few minutes, with seconds being a desirable goal.

It might be possible to require git commits and pushes, on a branch, of all software to be run in batch.
This could enable Jenkins or other CI mechanisms to build containers, whether automatically or manually triggered.

But being able to run even uncommitted (or at least unpushed) code is likely desirable.

.. Make in-text citations with: :cite:`bibkey`.
.. Uncomment to use citations
.. .. rubric:: References
.. 
.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
