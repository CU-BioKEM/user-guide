Alphafold
=========
**These instructions are not stable yet and will result in a ptaxs error.**
Alphafold is used to predict the structure of proteins using their sequence
alone. It does this through a machine learning approach that takes models of
similar sequences into consideration, but mainly relies on how likely 2
particular residues are to be close to each other in 3D space.

SBGrid packages Alphafold for us
(`see here <https://sbgrid.org/wiki/examples/alphafold2>`_)

To run Alphafold:

.. _Database:

Database
--------
The first thing we need to run Alphafold is the extensive database. This
database is over 2TB in size and takes a prohibitively long time to download on
RC infrastructure. I have downloaded it and place it in
``/pl/active/BioKEM/software/alphafolddb`` (you should be able to read this
location, but won't be able to update it.). **Don't attempt to download this
database on your own, use this one.**

.. _Input file:

Input file
----------
The only input you need to run Alphafold is your sequence(s) of interest in a
file in fasta format.

  #. Navigate to a directory in your PL and make a new directory for each fold
  you are attempting (``mkdir <dir_name>``)
  #. Create a fasta file (``touch <my_protein>.fa``)
  #. Edit fasta using nano, vim, or other.

    - You need to add your fasta header, which much contain a ``>`` followed by
    your protein name
    - You will then add your protein sequence in single letter codon format
    - If you are folding a multimer, add multiple entries to this file
    (``>``+name, sequence, ``>``+name, sequence, etc.)

    example fasta:

  .. code-block:: bash

    >T1083
    GAMGSEIEHIEEAIANAKTKADHERLVAHYEEEAKRLEKKSEEYQELAKVYKKITDVYPNIRSYMVLHYQNLTRRY
    KEAAEENRALAKLHHELAIVED

.. _Running alphafold:

Running alphafold
-----------------
To run alphafold, we will need to submit a job to ``sbatch`` requesting GPUs.
I've created a command that should handle this all for you called
``alphafold-predict``. If you've set up your environment correctly
:doc:`../configure.rst`, this should be in your path and will work if you are in a
``biokem-interactive`` session.

  #. Log on to OpenOnDemand :doc:`../logging_on.rst`
  #. Start an interactive session ``biokem-interactive``
  #. Make your :ref:Input
  #. Run ``alphafold-predict``

    .. code-block:: bash

      Alphafold script usage:
        alphafold-predict --monomer <fasta.fa>
        alphafold-predict --multimer <fasta.fa>

      If folding large >1,200aa, use:
        alphafold-predict --monomer <fasta.fa> --large
        alphafold-predict --multimer <fasta.fa> --large

This sbatch script that is actually being submitting for you:
``/projects/biokem/software/biokem/users/example_sbatch_scripts/alphafold/predict_monomer.q``
(There are few variations on this script in that fold for multimers and large
proteins, the alphafold-predict options will submit those when specified)

  .. code-block:: bash

  #!/bin/bash
  #SBATCH --partition=blanca
  #SBATCH --qos=preemptable
  #SBATCH --account=blanca-biokem
  #SBATCH --job-name=alphafold_predict
  #SBATCH --nodes=1
  #SBATCH --ntasks=16
  #SBATCH --mem=64gb
  #SBATCH --gres=gpu:1
  #SBATCH --time=24:00:00
  #SBATCH --output=/home/%u/slurmfiles_out/slurm_%j.out
  #SBATCH --error=/home/%u/slurmfiles_err/slurm_%j.err

  #Path to fasta file, needs each monomer as own chain
  FASTA=$1
  echo "Predicting monomer for file: $1"

  #Run this inside SBGrid environment
  source /programs/sbgrid.shrc

  #set to Alphafold 2.3.1 (database needs to be updated if changed)
  ALPHAFOLD_X=2.3.1
  DB='/pl/active/BioKEM/software/alphafolddb/'

  /programs/x86_64-linux/alphafold/${ALPHAFOLD_X}/bin.capsules/run_alphafold.py \
      --data_dir=${DB} \
      --output_dir=$(pwd) \
      --fasta_paths=${FASTA} \
      --max_template_date=2020-05-14 \
      --db_preset=full_dbs \
      --bfd_database_path=${DB}bfd/bfd_metaclust_clu_complete_id30_c90_final_seq.sorted_opt \
      --uniref30_database_path=${DB}uniclust30/uniclust30_2018_08/uniclust30_2018_08 \
      --uniref90_database_path=${DB}uniref90/uniref90.fasta \
      --mgnify_database_path=${DB}mgnify/mgy_clusters_2018_12.fa \
      --template_mmcif_dir=${DB}pdb_mmcif/mmcif_files \
      --obsolete_pdbs_path=${DB}pdb_mmcif/obsolete.dat \
      --use_gpu_relax=True \
      --model_preset=monomer \
      --pdb70_database_path=${DB}pdb70/pdb70

.. _Known errors:

Known errors
------------
Running Alphafold in this way (either for a monomer or multimer) will result in
the following error:

  .. code-block:: bash

    jaxlib.xla_extension.XlaRuntimeError: FAILED_PRECONDITION: Couldn't get
    ptxas version string: INTERNAL: Running ptxas --version returned 32512

This error has to do with a mismatch between a CUDA version and the NVIDIA
driver installed on the graphics card (`see here
<https://github.com/kalininalab/alphafold_non_docker/issues/26>`_)

I have tried forcing a different CUDA version, this doesn't seem to solve the
problem.

There also seems to be a way to suppress this error by not using the GPU, but
this will essentially make the program useless, so we need to fix this. Let me
know (Shawn) when you have a fix and I will update this documentation.
