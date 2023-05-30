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

.. _adim:

Lab admins
----------

Each lab should be assigned a lab admin.

  - Initial admin account in lab's CryoSPARC.
  - Rsponsible for creating new users within CryoSPARC.
  - Can execute ``cryosparcm-restart`` in order to restart lab's CryoSPARC instance.

.. _cryosparc tips:

Tips
----

Lanes
~~~~~

  - **Alpine**: RMACC (CU, CSU, ect.) general use cluster. This will submit to the ``aa100`` partition. Lots of A100's, but can have longer wait times. 24hr time limit.
  - **Blanca**: Buy-in condo cluster for CU. Mixed hardware, generally shorter wait times. 24hr time limit.
  - **Blanca-biokem**: The BioKEM owned partition of Blanca. 7 x RTX6000 and 2 x A100, we have priority. 7d time limit.
  - **Blanca-128gb**: General Blanca cluster, but will request 128GB of memory for each job. This will result in longer wait times, but may be necessary for jobs that run out of memory otherwise.

The VM running your installation of CryoSPARC can submit jobs to either Alpine
or Blanca. Although your jobs should start eventually wherever you submit, they
will likely start quickest and with the most resources on Blanca.

SSDs
~~~~

All nodes have an SSD attached, ranging from 256GB to TBs. CryoSPARC is able to
use the SSD if told to, but you could run out of space if running a large job.
If you want to use it, try it out. If it fails, disable the SSD and resubmit. In
my experience, enabling the SSD doesn't lead to a huge performance gain.

CryoSPARC Live
~~~~~~~~~~~~~~

`CryoSPARC Live <https://guide.cryosparc.com/live/about-cryosparc-live>`_ allows you to perform preprocessing tasks (motion correction, 
CTF estimation, particle picking, 2D classification, and Ab-initio reconstruction) as images are transfered into your PL. It can also 
be useful even if all your images are already in place, in that you can tweak parameters and see the results very quickly. To use Live, 
open CryoSPARC and click on the lightning bolt on the left panel, from there select a project (or make a new one), and follow the 
`guide <https://guide.cryosparc.com/live/about-cryosparc-live>`_. A few things to consider:

  - When selecting worker lanes, try ``blance-biokem`` or ``blanca``
  - Set ``Number of Preprocessing GPU Workers`` to 2 or more
  - Disable ``Use SSD``
  - Enable ``Enable continuous import``
  - ``Directory to watch`` is really the directory, not the file name with a wildcard as it normally is
  - ``File name wildcard filter`` set to ``*.eer`` (or ``*.mrc``, etc.)
  - Enable ``Search recursively``
  - You have to press the green ``Enable`` button, in order to be able to press ``Start session`` at the top

Topaz
~~~~~

Topaz is a neural net based picking algorithm. We will use the version installed 
through SBGrid. For ``Path to Topaz Executable`` use ``/programs/x86_64-linux/topaz/0.2.5_cu11/bin.capsules/topaz`` 
you can substitute ``0.2.5_cu11`` for another version, if available. 

A few things to consider:

  - Check out the `Topaz tutorial <https://guide.cryosparc.com/processing-data/all-job-types-in-cryosparc/deep-picking/topaz>`_ before starting
  - Train topaz on a small subset (10-50) exposures first, aiming for ~1000 particles
  - Training topaz on too many particles (>5000) will result in a job failure
  - After training a small set and seeing good results with particle picking, move on to full dataset


General
~~~~~~~

- Start with a subset of exposures to see what will work first
- Winnow down particles (using NCC and power in ``Inspect particles``) before starting 2D classification
- Submit to the general Blanca lane
- Disable SSDs

Bugs
~~~~

#. Firefox crashes
  - In some cases Firefox will fail to open for specific users on the viz node. 
  - Use the ``resetfirefox`` command.
  - You can use the ``alias`` command on the cluster to find your lab's CryoSPARC IP 
    address. Simply paste this into your local internet broswer and continue using CryoSPARC.