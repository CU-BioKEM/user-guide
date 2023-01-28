SBATCH scripts
==============

*Many GUI based programs including RELION and CryoSPARC will submit these
scripts for you.*

SBATCH scripts are shell scripts submitted to the SLURM manager using the command:

``sbatch <script>``

The first part of the script is used to tell the SLURM manager what resources you will need for your job and other
informaiton about how to run it. The rest of the script is were you will pass the commands to run your job.

To specify that a job runs on the BioKEM nodes you need set these SBATCH variables:

```bash
#!/bin/bash
#SBATCH --partition=blanca-biokem    #submit to any nodes owned by biokem
#SBATCH --qos=blanca-biokem          #use biokem priorty
#SBATCH --account=blanca-biokem      #need an account to submit jobs, don't change
#SBATCH --job-name=my_biokem_job     #change this to something descriptive
#SBATCH --nodes=1                    #number of nodes to allocate, never need to change
#SBATCH --gres=gpu:0                 #number of gpus, set to 0 if not needed
#SBATCH --ntasks=1                   #number of cpus to allocate
#SBATCH --mem=8gb                    #how much memory to allocate
#SBATCH --time=24:00:00              #time limit after which job will die 24hrs max on blanca or 168hrs if biokem
#SBATCH --output=/home/%u/slurmfiles_out/slurm_%j.out    #output file of job - useful for checking progress of job
#SBATCH --error=/home/%u/slurmfiles_err/slurm_%j.err     #error file of job - useful for debugging

#load any modules you need here, the job will be submitted with current environment (i.e. SBGrid shell, if loaded)
module load <modules>

#execute commands here
<commands>
```

To specify that a job runs on any Blanca nodes you need set these SBATCH variables:

``
#!/bin/bash
#SBATCH --partition=blanca           #submit to any nodes in blanca
#SBATCH --qos=preemptable            #job can be interrupted, but will be requeued
#SBATCH --account=blanca-biokem      #need an account to submit jobs, don't change
#SBATCH --job-name=my_blanca_job     #change this to something descriptive
#SBATCH --nodes=1                    #number of nodes to allocate, never need to change
#SBATCH --gres=gpu:0                 #number of gpus, set to 0 if not needed
#SBATCH --ntasks=1                   #number of cpus to allocate
#SBATCH --mem=8gb                    #how much memory to allocate
#SBATCH --time=24:00:00              #time limit after which job will die 24hrs max on blanca or 168hrs if biokem
#SBATCH --output=/home/%u/slurmfiles_out/slurm_%j.out    #output file of job - useful for checking progress of job
#SBATCH --error=/home/%u/slurmfiles_err/slurm_%j.err     #error file of job - useful for debugging

#load any modules you need here, the job will be submitted with current environment (i.e. SBGrid shell, if loaded)
module load <modules>

#execute commands here
<commands>
``

To request a GPU:

Update this line with the number of GPUs ``#SBATCH --gres=gpu:1`

To request a specific GPU:

``#SBATCH --gres=gpu:a100:1``

Where ``a100`` is ``a100,rtx6000,v100,etc.``

**Always be realistic about how many resource your job will need to run. This
will ensure your job runs quickly and doesn't hog resources that others might need.**

**Another thing to remember is that the environment in which you submitted your** ``sbatch <script>`` \
**command will be passed through to your job. You can always specify your environments, vairables, \
or load modules before your running commands to ensure that it will run in the proper environment every time.**
