ChimeraX
========

ChimeraX is 3D structural visualization tool that renders publication quality figures
and movies. It also has the capability to run VR (although not on the cluster), 
molecular dynamics simulations, and build molecular models.

There are 3 ways to access ChimeraX on the cluster:

#. *Preferred*. Viz node (all versions):

    #. When starting a viz node job, use more CPUs (maybe 8)
    #. Load Sbgrid and call ChimeraX

      .. code-block:: bash

        sbgrid
        chimerax

#. *Old* Viz node (only RC installed v1.3):

    #. Log onto a viz node
    #. Load module and start

        .. code-block:: bash
          
          ml chimerax
          chimerax
    
#. Compute node (only v1.3)

    #. In an sbatch script or a interactive job:

        .. code-block:: bash

          sbgrid
          chimerax
    

Chimera
-------
Chimera is the older version of ChimeraX. In most ways ChimeraX is cleaner and produces
much nicer figures, but Chimera has more built out features and can be useful for 
certain applications.

To use, you will need to be on a compute node (interactive job or in an sbatch script),
but then it's the same as using ChimeraX thourgh SBGrid:

    .. code-block:: bash

        sbgrid
        chimera