Environments
============

If you've enabled the SBGrid/BioKEM environment through the
:doc:`configure` you should now have the following enabled:

BioKEM environment
~~~~~~~~~~~~~~~~~~

Aliases
-------
- ``blanca``: load the Blanca SLURM environment when logged into a login node (this could be loaded by other commands).
- ``sbgrid``: source the SBGrid environment, putting all the SBGrid executables in your path.

Executables
-----------
- ``biokem/blanca/alpine-interactive``: starts an interactive session with X forwarding, use this launch jobs. You can add the following arguments, if you want to request additional resources:

    - ``-c`` or ``--cpus`` # of CPUs
    - ``-g`` or ``--gpus`` # of GPUs
    - ``-m`` or ``--mem``  # of GB of memory
    - ``-x`` additional string to add an sbatch argument

Variables
---------
- Various RELION environmental variables
- ``MESA_GL_VERSION_OVERRIDE=3.3``: allows SBGrid's ChimeraX to function.

Non-SBGrid environment
~~~~~~~~~~~~~~~~~~~~~~

Aliases
-------
- ``blanca``: load the Blanca SLURM environment when logged into a login node (this could be loaded by other commands).

Modules
-------
- BioKEM facility installed IMOD, PEET, and RELION modules

Executables
-----------
- ``biokem/blanca/alpine-interactive``: starts an interactive session with X forwarding, use this launch jobs. You can add the following arguments, if you want to request additional resources:

    - ``-c`` or ``--cpus`` # of CPUs
    - ``-g`` or ``--gpus`` # of GPUs
    - ``-m`` or ``--mem``  # of GB of memory
    - ``-x`` additional string to add an sbatch argument

Variables
---------
- Various RELION environmental variables
- IMOD configuration files
