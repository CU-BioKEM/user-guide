Interactive jobs
================

In addition to running ``sbatch`` scripts, you may allocate resources and run jobs interactively.

To do this:
    - Spawn an interactive job with the right resources:

      .. code-block:: bash

        biokem-interactive -c <# of cpus> -g <# of gpus> -m <# of GB of memory> -x <additional sbatch arguments>

      For example, if you wanted to run IMOD interactively with a GPU, you might use something like this:

      .. code-block:: bash

        biokem-interactive -c 8 -g 1 -m 64

    - Once the interactive job is spawned, your terminal will be taken to a compute node where you can compute \
      as you would on a local workstation.
    - You must keep the ternminal alive during your interactive job (make sure you spawned your OpenOnDemand \
      session for a long enough period).
    - Make sure to end the job using the ``exit`` command when your job is complete to free up resources for \
      others.
