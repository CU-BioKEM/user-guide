Configuring your environment
============================

**Now that you have logged onto Blanca, you can start computing.  **If you plan on doing a lot of computing, consider purchasing an SBGrid license. Technically, each lab needs their own license.** 

#. The first time you log onto blanca, run the following command to enable our :doc:`environments/biokem_environment` at login:

    ``echo 'source /projects/biokem/software/biokem/users/src/biokem_environment.src' >> ~/.bashrc``

#. If you just ran the command above, close your terminal and open a new one.

#. Run the ``biokem-interactive`` or ``blanca-interactive 1 0`` commands to start an interactive node that will allow you to use software.

#. Run ``sbgrid`` to start the environment.

#. All SBGrid install programs should now be accessible by simply executing the desired executable.

#. To access RC installed programs use the ``module avail`` to show all available modules and ``module load <name>`` to load them.

#. Please submit your jobs through the SLURM manager using ``sbatch`` (RELION and other programs handle this through the GUI).
