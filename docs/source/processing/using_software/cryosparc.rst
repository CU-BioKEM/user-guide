CryoSPARC
=========

*The SLURM token allowing submission to all CryoSPARC instances setup through
BioKEM will expire in early 2028. This will prevent all instances from
submitting jobs to the cluster.*

CryoSPARC is a webapp-based cryoEM data processing suite devolped by 
Structura Biotechnology. Although proprietary, licenses are available 
to academic users for free.

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
  - Responsible for creating new users within CryoSPARC.
  - Can execute ``cryosparcm-restart`` to restart lab's CryoSPARC instance.

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

  - When selecting worker lanes, try ``blance-biokem`` or ``blanca``.
  - Set ``Number of Preprocessing GPU Workers`` to 2 or more.
  - Disable ``Use SSD``
  - Enable ``Enable continuous import``
  - ``Directory to watch`` is really the directory, not the file name with a wildcard as it normally is.
  - ``File name wildcard filter`` set to ``*.eer`` (or ``*.mrc``, etc.)
  - Enable ``Search recursively``
  - You have to press the green ``Enable`` button, in order to be able to press ``Start session`` at the top.
  - **Make sure to pause or end your session after you are done, as it will continue to allocate nodes until it's done.** 

Topaz
~~~~~

Topaz is a neural net based picking algorithm. We will use the version installed 
through SBGrid. For ``Path to Topaz Executable`` use ``/programs/x86_64-linux/topaz/0.2.5_cu11/bin.capsules/topaz`` 
you can substitute ``0.2.5_cu11`` for another version, if available. 

A few things to consider:

  - **Topaz needs to be run on a blanca lane (because of SBGrid)**
  - Check out the `Topaz tutorial <https://guide.cryosparc.com/processing-data/all-job-types-in-cryosparc/deep-picking/topaz>`_ before starting
  - Train topaz on a small subset (10-50) exposures first, aiming for ~1000 particles.
  - Training topaz on too many particles (>5000) will result in a job failure.
  - After training a small set and seeing good results with particle picking, move on to full dataset.

Fourier cropping during motion correction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Data collect in super-reolution mode is often too large to be practicle for most job types to deal with. 
One way to reduce the image size is to "bin" it, which removes a certain sampling of pixels. The downside of 
this approach is that lots of information, especially high resolution data, is lost. To reduce this effect, 
CryoSPARC allows you to perform a Fourier crop after motion correction, which attempts to preserve more 
high frequency information (`see here for an explainer <https://blog.posertinlab.com/posts/2022-10-20-fourier-cropping/https://blog.posertinlab.com/posts/2022-10-20-fourier-cropping/>`_)

The downside of this operation is that it is memory intesive, especially with super resolution images. Essentially,
each CPU allocated needs to read the images it's working on into memory. Unfortunately, the way CryoSPARC 
allocates memory and number of CPUs for motion correction is based on number of GPUs allocated and not based on 
the actually memory required to do the operation, which is dependent on image size. To get around this, we 
can make a special lane which allocates lots of memory, while limiting the number of CPUs allocated. 

  - ``64GB RAM``
  - ``8 CPUs``
  - ``1 GPU`` (I haven't test whether more GPUs will work or not)

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
  - If that doesn't work try ``rm -rf ~/.mozilla`` (this will remove your cookies and stored logins).
  - You can use the ``alias`` command on the cluster to find your lab's CryoSPARC IP 
    address. Simply paste this into your local internet broswer and continue using CryoSPARC.