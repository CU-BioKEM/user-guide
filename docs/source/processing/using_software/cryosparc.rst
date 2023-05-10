CryoSPARC
=========

*The SLURM token allowing submission to all CryoSPARC instances setup through
BioKEM will expire in early 2028. This will prevent all instances from
submitting jobs to the cluster.*

.. _General users:

General users
-------------
Once your lab has the BioKEM IT person set up a CryoSPARC installation, all
you need to do to run it is:

  #. Log on to OpenOnDemand (:doc:`../logging_on`)
  #. Open terminal
  #. Run the command ``cryosparc``

Do the same for CryoSPARCLive, but then click on the lightning bolt. (See
:doc:`../otf_motion_correction`)

.. _Lab admins:

Lab admins
----------
You should be able to run ``cryosparcm restart`` as normal, but keep in mind
that this setup is more complex than a standalone workstation and the problems
are also.


.. _cryosparc tips:

Tips
----

Lanes
~~~~~

  - **Alpine**: RMACC (CU, CSU, ect.) general use cluster. This will submit to the ``aa100`` partition. Lots of A100's, but can have longer wait times. 24hr time limit.
  - **Blanca**: Buy-in condo cluster for CU. Mixed hardware, generally shorter wait times. 24hr time limit.
  - **Blanca-biokem**: The BioKEM owned partition of Blanca. 7 x RTX6000 and 2 x A100, we have priority. 7d time limit.

The VM running your installation of CryoSPARC can submit jobs to either Alpine
or Blanca. Although your jobs should start eventually wherever you submit, they
will likely start quickest and with the most resources on Blanca.

SSDs
~~~~

All nodes have an SSD attached, ranging from 256GB to TBs. CryoSPARC is able to
use the SSD if told to, but you could run out of space if running a large job.
If you want to use it, try it out. If it fails, disable the SSD and resubmit. In
my experience, enabling the SSD doesn't lead to a huge performance gain.

General
~~~~~~~

- Start with a subset of exposures to see what will work first
- Winnow down particles (using NCC and power in ``Inspect particles``) before starting 2D classification
- Submit to the general Blanca lane
- Disable SSDs
