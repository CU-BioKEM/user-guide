Computing on blanca-biokem
==========================

Goal
----
This guide will introduce new and existing users of the `CU-Boulder BioKEM
facility <https://www.colorado.edu/facility/biokem/>`_ to processing EM data on
GPU compute nodes hosted on Blanca.

Overview
--------
The BioKEM computing server consists of three main parts: Petalibrary, Blanca,
and SBGrid. In order to use the server, you will need to gain access to all
three of these resources. Information on how to do this can be found in the
Getting started section. In brief, you will host your dataset on Petalibrary,
process it on GPUs within the Blanca computing cluster using software managed
through SBGrid.

Getting started
---------------

Petalibrary
~~~~~~~~~~~
`Petalibrary <https://www.colorado.edu/rc/resources/petalibrary>`_ is CU's
Research Computing (RC) files storage system. This service can be used to safely
and securely store 100's of TBs of data for a modest cost. This system is well
backed up, so you don't have to worry about losing valuable data. The other
important aspect of this service is that you can securely mount it to other RC
resources like Blanca for super fast file transfer.

You will need to request and allocation for you lab and have RC create a
biokem-deposit folder that can be used to deposit images taken from the Krios.
See https://www.colorado.edu/rc/resources/petalibrary for more information and
email rc-help@colorado.edu to request and allocation. We recommend requesting an
allocation of about ~20TB/actively processing dataset. This service could also
be a good long term storage option for already processed datasets.

Blanca
~~~~~~
`Blanca <https://www.colorado.edu/rc/resources/blanca>`_ is RC's condo computing
cluster. This cluster allows users to purchase their own computing nodes that RC
will then house, setup, and manage. Users have priority on the nodes they own,
but have access to use other users nodes when they are available. The BioKEM
facility owns two types of nodes: one reserved for on the fly motion correction
and the other for users of the facility to process EM data. The physical space
we are allotted can house up to 10 processing nodes in addition to the on the
fly node. These nodes are purchased by individual labs, but are then shared by
all members of the BioKEM facility. Each node will consist of a number of GPUs
(typically 4) and CPUs, as well as RAM to run jobs. All jobs ran in the cluster
are submitted by a workload manager called SLURM.

SLURM
~~~~~
`SLURM <https://slurm.schedmd.com>`_ is the management system many computing
clusters, including Blanca use to run jobs. It allows multiple users to use the
same computing resources by distributing jobs across resources and scheduling
jobs to start when resources become available. The main benefits to us of using
SLURM are:

   - Maximize utilization of nodes by running 24/7
   - Avoid single users from hogging resources
   - Create easy to replicate workflows
   - Allows us to use non-BioKEM nodes when they are available

All of these will bring the cost of high performance computing per lab down
drastically, while working within a consistent compute environment that should
be easier for the community to troubleshoot than working in individual labs.

SBGrid
~~~~~~
To manage all of the software necessary for processing EM data, we are using a
software manager called `SBGrid <https://sbgrid.org>`_. This service allows us to
maintain multiple versions of software, as well as easily install and update new
software. General members of the EM community may use a basic set of software
under the facility's license including:

   - crYOLO
   - CTFFind
   - cryoDRGN
   - deepEMhancer
   - MotionCor2
   - PyEM
   - Relion

Labs interested in using the suite of ~400 programs must purchase a lab specific
license from SBGrid, we will then grant lab members access to these
applications.

Commercial users are limited to a few preprocessing applications without an
additional license.

Running software
~~~~~~~~~~~~~~~~
In order to run software on BioKEM's Blanca server:
   - Request access to Blanca through `Research Computing <https://rcamp.rc.colorado.edu/accounts/account-request/create/organization>`_
   - Request a Petalibrary allocation (~20TB/active project) by emailing
   rc-help@colorado.edu **Make sure to have them create a 'biokem-deposit'
   folder for your lab to deposit Krios images into**
   - Purchase an SBGrid license for your lab
       - Labs without an SBgrid license will have minimal access to software.
       - Labs with their own SBgrid license will be able to access the full stack of software, without limitation.
