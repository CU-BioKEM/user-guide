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
- ``biokem-interactive``: starts an interactive session with X forwarding on a BioKEM node, use this launch jobs. This command only allocates a single CPU with no GPUs.
- ``blanca-interactive``: starts an interactive session that is preemptable on a general blanca node. This command takes 2 arguments:

    - # of CPUs as the first argument
    - # of GPUs as the second argument

Variables
---------
