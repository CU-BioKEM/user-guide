SBGrid Setup 
============

- All Blanca and viz nodes see ``/programs``
- This is a symlink to ``/projects/biokem/software/sbgrid/2.4.0``
- Inside of which everything is symlinked to ``/pl/active/BioKEM/software/sbgrid/``

We could probably remove a level of convolution here, but it seems to work.
We could also set up a cron job to automate the updating of SBGrid, but nodes
usually run state-less, which messes this up (could be done from a CryoSPARC VM).
Currently, there is no security around who can use the SBGrid software, we could implemet
a usergroup if we need to. Only users in the ``biokem-facility`` group can update.

To update, run the alias ``sbgrid-update`` from Blanca, this submits an sbatch job to update.

