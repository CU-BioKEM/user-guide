BioKEM environment
==================

If you've enabled the SBGrid/BioKEM environment through the :doc:`../using_software` you should now have the following enabled:

Aliases
-------
- ``blanca``: load the Blanca SLURM environment when logged into a login node (this could be loaded by other commands).
- ``sbgrid``: source the SBGrid environment, putting all the SBGrid executables in your path.

Executables
-----------
- ``biokem/blanca/alpine-interactive``: starts an interactive session with X forwarding, use this launch jobs. This command only allocates a single CPU with no GPUs.
- ``biokem/blanca/alpine-custom``: starts an interactive session that takes 2 additional arguments:

    - # of CPUs as the first argument
    - # of GPUs as the second argument

Variables
---------
- Various RELION environmental variables
- ``MESA_GL_VERSION_OVERRIDE=3.3``: allows SBGrid's ChimeraX to function.
