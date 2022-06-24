Using software
==============

The Blanca cluster is managed by a `SLURM <https://slurm.schedmd.com>`__ system, which queues jobs from all users 
and runs them when resources become available. There are two ways to run software on the cluster:

    - Using :ref:`SBATCH scripts`
    - :ref:`Interactively` by allocating resources beforehand

For various reasons, submitting an ``sbatch`` script is the preferred method.  

.. _SBATCH scripts:

SBATCH scripts 
--------------

Many GUI based programs including RELION and CryoSPARC will submit these scripts for you.

SBATCH scripts are shell scripts submitted to the SLURM manager using the command ``sbatch <script>``. The first 
part of the script is used to tell the SLURM manager what resources you will need for your job and other 
informaiton about how to run it. For example:


The rest of the script is were you will pass the commands to run your job. For example:


To specify that a job runs on the BioKEM nodes you need set these SBATCH variables:

To specify that a job runs on any Blanca nodes you need set these SBATCH variables:

To request a specific GPU:


**Always be realistic about how many resource your job will need to run. This
will ensure your job runs quickly and doesn't hog resources that others might need.**

**Another thing to remember is that the environment in which you submitted your** ``sbatch <script>`` \
**command will be passed through to your job. You can always specify your environments, vairables, \
or load modules before your running commands to ensure that it will run in the proper environment every time.** 


.. _Interactively:

Interactively
-------------
