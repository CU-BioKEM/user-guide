Non-SBGrid environment
======================

Aliases
-------
- ``blanca``: load the Blanca SLURM environment when logged into a login node (this could be loaded by other commands).

Modules
-------
- BioKEM facility installed IMOD, PEET, and RELION modules

Executables
-----------
- ``biokem/blanca/alpine-interactive``: starts an interactive session with X forwarding, use this launch jobs. This command only allocates a single CPU with no GPUs.
- ``biokem/blanca/alpine-custom``: starts an interactive session that takes 2 additional arguments:

    - # of CPUs as the first argument
    - # of GPUs as the second argument

Variables
---------
