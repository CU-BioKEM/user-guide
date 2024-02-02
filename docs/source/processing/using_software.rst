Using software
==============

The Blanca cluster is managed by a `SLURM <https://slurm.schedmd.com>`_ system, which queues jobs from all users 
and runs them when resources become available. There are two ways to run software on the cluster:

.. toctree::
   :maxdepth: 1

   using_software/sbatch
   using_software/interactive

For various reasons, submitting an ``sbatch`` script is the preferred method.

SBGrid
======

Excluding CryoSPARC and a few special cases, all of the BioKEM supported software is installed and updated 
through `SBGrid <https://sbgrid.org/>`_. Our SBGrid configuration includes all versions of every title 
currently supported by SBGrid. If you'd like to see which versions of a program are available:

      .. code-block:: bash

        sbgrid
        sbgrid-info -L <name of title>

The output will tell you how to temporarily change the version of the title you are using. 

To change your default version add that variable to your SBGrid configuration file found at ``~/.sbgrid.conf``
(if the file doesn't exist, simply create it and supply it with the variables you want).

Specific software
=================

More information on running specific programs on Blanca:

.. toctree::
   :maxdepth: 1

   using_software/alphafold
   using_software/eman2
   using_software/chimera
   using_software/cryosparc
   using_software/imod
   using_software/relion
