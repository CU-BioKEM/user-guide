Alphafold
=========
Alphafold is used to predict the structure of proteins using their sequence
alone. It does this through a machine learning approach that takes models of
similar sequences into consideration, but mainly relies on how likely 2
particular residues are to be close to each other in 3D space.

SBGrid packages Alphafold for us
(`see here <https://sbgrid.org/wiki/examples/alphafold2>`_)

Running alphafold
-----------------
If you've set up your environment correctly (see :doc:`../configure`),
``alphafold-predict`` should be in your path and will work if you are in a
``biokem-interactive`` session.

  #. Log on to OpenOnDemand (see :doc:`../logging_on`)
  #. Logon to a login node:

      .. code-block:: bash

        ssh login10

  #. Start an interactive session:

      .. code-block:: bash

        biokem-interactive

  #. Navigate to your working directory (usually ``/pl/active/<yourlab>``)
  #. Make your :ref:`Input file`
  #. Run:

      .. code-block:: bash

        alphafold-predict <your_fasta.fa>

Check the status of your job:

    .. code-block:: bash

      squeue -u $USER

Check the output of alphafold (it writes to the error file):

**If it was the most recent job you submitted, you can omit the jobid argument.**

    .. code-block:: bash

      slurm-err <jobid>

.. _Input file:

Input file
~~~~~~~~~~
The only input you need to run Alphafold is your sequence(s) of interest in a
file in fasta format.

  #. Navigate to a directory in your PL and make a new directory for each fold you are attempting (``mkdir <dir_name>``)
  #. Create a fasta file (``touch <my_protein>.fa``)
  #. Edit fasta using nano, vim, or other.

    - You need to add your fasta header, which much contain a ``>`` followed by your protein name
    - You will then add your protein sequence in single letter codon format
    - If you are folding a multimer, add multiple entries to this file (``>`` + name, sequence, ``>`` + name, sequence, etc.)

    example fasta:

        .. code-block:: bash

          >T1083
          GAMGSEIEHIEEAIANAKTKADHERLVAHYEEEAKRLEKKSEEYQELAKVYKKITDVYPNIRSYMVLHYQNLTRRY
          KEAAEENRALAKLHHELAIVED

    example multimer fasta:

        .. code-block:: bash

          >T1083
          GAMGSEIEHIEEAIANAKTKADHERLVAHYEEEAKRLEKKSEEYQELAKVYKKITDVYPNIRSYMVLHYQNLTRRY
          KEAAEENRALAKLHHELAIVED
          >T1083
          GAMGSEIEHIEEAIANAKTKADHERLVAHYEEEAKRLEKKSEEYQELAKVYKKITDVYPNIRSYMVLHYQNLTRRY
          KEAAEENRALAKLHHELAIVED

.. _Database:

Database
~~~~~~~~
The Alphafold database is over 2TB in size and takes a prohibitively long time
to download on RC infrastructure. I have downloaded it and place it in
``/pl/active/BioKEM/software/alphafolddb`` (you should be able to read this
location, but won't be able to update it.). **Don't attempt to download this
database on your own, use this one (the script does this for you).**

.. Alpha submission:

Submission script
~~~~~~~~~~~~~~~~~
This is the sbatch script that is actually being submitting for you:
``/projects/biokem/software/biokem/users/example_sbatch_scripts/alphafold/predict_monomer.q``
(There are few variations on this script in that folder for multimers and large
proteins, alphafold-predict will submit those for you).

  .. code-block:: bash

    #!/bin/bash
    #SBATCH --partition=blanca
    #SBATCH --qos=preemptable
    #SBATCH --account=blanca-biokem
    #SBATCH --job-name=alphafold_predict
    #SBATCH --nodes=1
    #SBATCH --ntasks=10
    #SBATCH --mem=64gb
    #SBATCH --gres=gpu:a100:1
    #SBATCH --time=24:00:00
    #SBATCH --output=/home/%u/slurmfiles_out/slurm_%j.out
    #SBATCH --error=/home/%u/slurmfiles_err/slurm_%j.err

    #Path to fasta file, needs each monomer as own chain
    FASTA=$1
    echo "Predicting monomer for file: ${FASTA}"

    #Run this inside SBGrid environment
    PATH=$PATH:/curc/sw/cuda/11.2/bin
    _PTXAS=userpath
    TF_FORCE_UNIFIED_MEMORY=1
    source /programs/sbgrid.shrc

    #set to Alphafold 2.3.2 (database needs to be updated if changed)
    ALPHAFOLD_X=2.3.2
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

.. GPU timings:

GPU timings
~~~~~~~~~~~

I tested the speed different GPU configurations on Blanca using a small multimer
system (2 chains, ~140aa) and found that A40s, followed by A100s were the fastest.
The scripts in "alphafold-predict" will submit to A100s.

      A40 1x : 1670s
      A40 2x : 1670s
      A100 2x : 1785s
      A100 1x : 1851s
      Rtx6000 2x : 1895s
      Rtx6000 1x : 1907s
      V100 1x : 2354s
      V100 2x : 2390s
      T4 1x : 2890s
      CPU 16x : 7802s
      P100 1x : 2513s
      CPU 8x : 10513s
      CPU 32x : 11981s
      P100 2x : 2989s


.. _Known errors:

Known errors
------------
