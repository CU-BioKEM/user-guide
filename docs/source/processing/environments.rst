BioKEM environment
==================

If you've enabled the BioKEM environment through the
:doc:`configure` you should now have the following:

Aliases
-------
- ``blanca`` : load the Blanca SLURM environment when logged into a login node (this could be loaded by other commands).
- ``sbgrid`` : starts SBGrid environment when on Blanca
- ``scratch`` : takes you to your scratch directory 
- ``s`` : shows you your jobs (on the cluster you are on)
- ``blanca-gpus`` : shows all GPUs on blanca (need to be on a blanca node)

Executables
-----------
- ``biokem/blanca/alpine-interactive``: starts an interactive session with X forwarding, use this launch jobs. You can add the following arguments, if you want to request additional resources:

    - ``-c`` or ``--cpus`` # of CPUs
    - ``-g`` or ``--gpus`` # of GPUs
    - ``-m`` or ``--mem``  # of GB of memory
    - ``-x`` additional string to add an sbatch argument

- ``alphafold-predict`` : submits an AlphaFold job
- ``slurm-err`` : reads out latest SLURM error file, can also take a job number as argument
- ``slurm-out`` : reads out latest SLURM out file, can also take a job number as argument


Variables
---------
- Various RELION environmental variables
- ``MESA_GL_VERSION_OVERRIDE=3.3``: allows SBGrid's ChimeraX to function.
