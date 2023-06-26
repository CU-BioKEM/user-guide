RELION
======

RELION is an open source cryoEM data processing suite.
It is implemented here through SBGrid. 

Starting up
-----------
There are two ways to run RELION on the Blanca cluster.

    - :ref:`sbatch submission` **Preferred**
    - :ref:`interactively`

.. _sbatch submission:

Sbatch submission
~~~~~~~~~~~~~~~~~
RELION is built with HPC integration in mind, so it can 
submit sbatch scripts for you and makes them easy to write
inside the GUI. (underlying templates are found at 
``/projects/biokem/software/biokem/users/src``). To start, allocate
an interactive job with minimal resources to run the GUI on Blanca,
start the GUI, and then ``Submit to queue``:

    .. code-block:: bash
      
      biokem-interactive
      cd /path/to/data/folder
      sbgrid
      relion

Now in the ``Running`` tab:

    - Set ``Submit to queue`` to ``Yes``
    - Defaults should be auto populated (``Walltime`` can be up to ``168:00:00``)
    - If you want to submit to Blana at large:
        
        - ``Queue name`` = ``blanca``
        - ``QoS`` = ``preemtable``
        - ``Account`` = ``blanca-biokem``
        - ``Walltime`` = ``24:00:00`` (or less)

You can submit multiple jobs at the same time using this method. 
You can use the GUI to moniter the output of each job.

.. _interactively:

Interactively
~~~~~~~~~~~~~
When starting interactively, you must allocated all of the 
resources you need before starting the GUI. Example:

    .. code-block:: bash
      
      biokem-interactive -c 24 -g 1 -m 48
      cd /path/to/data/folder
      sbgrid
      relion

The drawbacks with this method are:

    - Need to know how many resources you will use beforehand
    - Only way to allocate more resources is to exit screen and start over
    - Need to keep terminal alive while processing (24hr limit)
    - Have to wait until resources become available to start jobs
    - Sequesters idle resources from other users 

.. _RELION OTF:

RELION OTF
~~~~~~~~~~
Similar to CryoSPARC's Live function, RELION can also preprocess 
data as it is being transferred into your folder using `Schedules`
(See `here <https://relion.readthedocs.io/en/release-3.1/Reference/Schedules.html>`_).


