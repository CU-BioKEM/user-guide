Using software
==============

**Now that you have logged onto Blanca, you can start computing. Use the** :ref:`SBGrid` **instructions if your lab has an SBGrid license or the** :ref:`Non-SBGrid` **instructions if your lab doesn't.**

.. _SBGrid:

SBgrid
------

#. The first time you log onto blanca, run the following command to enable our :doc:`environments/biokem_environment` at login:

    ``echo 'source /projects/biokem/software/biokem/users/src/biokem_environment.src' >> ~/.bashrc``

#. If you just ran the command above, close your terminal and open a new one.

#. Run the ``biokem-interactive`` or ``blanca-interactive 1 0`` commands to start an interactive node that will allow you to use software.

#. Run ``sbgrid`` to start the environment.

#. All SBGrid install programs should now be accessible by simply executing the desired executable.

#. To access RC installed programs use the ``module avail`` to show all available modules and ``module load <name>`` to load them.

#. Please submit your jobs through the SLURM manager using ``sbatch`` (RELION and other programs handle this through the GUI).

.. _Non-SBGrid:

Non-SBGrid
----------

#. The first time you log onto blanca, run the following command to enable our :doc:`environments/non_sbgrid_environment` at login:

    ``echo 'source /projects/biokem/software/biokem/users/src/non_sbgrid_environment.src' >> ~/.bashrc``

#. If you just ran the command above, close your terminal and open a new one.

#. Run the ``biokem-interactive`` or ``blanca-interactive 1 0`` commands to start an interactive node that will allow you to use software.

#. To access BioKEM or RC installed programs use the ``module avail`` to show all available modules and ``module load <name>`` to load them.

#. Please submit your jobs through the SLURM manager using ``sbatch`` (RELION and other programs handle this through the GUI).
