.. _Non-SBGrid:

Non-SBGrid
----------

#. The first time you log onto blanca, run the following command to enable our :doc:`environments/non_sbgrid_environment` at login:

    ``echo 'source /projects/biokem/software/biokem/users/src/non_sbgrid_environment.src' >> ~/.bashrc``

#. If you just ran the command above, close your terminal and open a new one.

#. Run the ``biokem-interactive`` or ``blanca-interactive 1 0`` commands to start an interactive node that will allow you to use software.

#. To access BioKEM or RC installed programs use the ``module avail`` to show all available modules and ``module load <name>`` to load them.

#. Please submit your jobs through the SLURM manager using ``sbatch`` (RELION and other programs handle this through the GUI).
