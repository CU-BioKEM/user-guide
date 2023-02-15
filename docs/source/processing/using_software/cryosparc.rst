Cryosparc
=========

.. _General users:

*The SLURM token allowing submission to all CryoSPARC instances setup through
BioKEM will expire in early 2028. This will prevent all instances from
submitting jobs to the cluster.*

General users
-------------
Once your lab has set up a Cryosparc installation with the BioKEM IT person, all
you need to do to run it is:

  #. Log on to OpenOnDemand (:doc:`../logging_on`)
  #. Open terminal
  #. Run the command ``cryosparc``

.. _Lab admins:

Lab admins
----------
You should be able to run ``cryosparcm`` commands as normal, although I have not
tested updating. Please don't attempt to update until I have tested it out.

.. _Setup:

Setup (facility use only)
-------------------------
**Do not attempt to do this on your own.** Cryosparc is a pain in the butt to
get to play with CURC infrastructure, but it can be done. Here, we will:

   - :ref:`VM`
   - :ref:`SLURM integration`
   - :ref:`Mount PL`
   - :ref:`Cryomaster`
   - :ref:`Cryoworker`
   - :ref:`Cryo aliases`

.. _VM:

Create a Cryosparc VM
^^^^^^^^^^^^^^^^^^^^^
We will spin up a small VM to run the 'master' instance of Cryosparc on CURC's
CUmulus cloud service. Currently, only the BioKEM IT admin has access to this
allocaion. We will follow these `instructions
<https://curc.readthedocs.io/en/latest/tutorials/cumulus1.html>`_.

#. Go to `OpenStack <https://cumulus.rc.colorado.edu/auth/login/?next=/>`_
#. Instances > `Launch Instance`

   - Details > Add name
   - Source > Ubuntu 20.04 LTS
   - Set volume to 16GB
   - Flavor > m5.large
   - Networks > projectnet2023-private
   - Security Groups > hpc-ssh, default, ssh-restricted, icmp, rfc-1918
   - Key Pair > add BioKEM global user's RSA key**

#. Associate Floating IP

   - ``+``
   - Pool > scinet-internal
   - Allocate IP
   - Associate


.. _SLURM integration:

Integrate SLURM
^^^^^^^^^^^^^^^
In order to submit jobs to Alpine's SLURM environment, we need to install the
rigth version of SLURM, import Alpine's slurm config, and set up a user that has
permission to submit jobs. We will be using a variation of `this <https://curc.readthedocs.io/en/latest/cloud/slurm-integration.html>`_.

#. Log on to the VM ``ssh -o KexAlgorithms=ecdh-sha2-nistp521 ubuntu@<IP>``

    .. code-block:: bash

      sudo apt-get update
      sudo apt install -y libmysqlclient-dev libjwt-dev munge gcc make

#. Check SLURM version (on RC):

    .. code-block:: bash

      ml slurm/alpine
      sbatch --version

#. On VM (make sure to clone correct slurm):

    .. code-block:: bash

      cd /opt
      sudo git clone -b slurm-22.05 https://github.com/SchedMD/slurm.git
      cd slurm
      sudo ./configure --with-jwt --disable-dependency-tracking
      sudo make && sudo make install
      sudo mkdir -p /etc/slurm
      cd /etc/slurm

    .. code-block:: bash

      sudo scp <user>@login.rc.colorado.edu:/curc/slurm/alpine/etc/slurm.conf .
      sudo nano slurm.conf

    .. code-block:: bash

      ControlMachine=alpine-slurmctl1.rc.int.colorado.edu
      BackupController=alpine-slurmctl2.rc.int.colorado.edu

#. Edit ``/etc/default/useradd`` -> ``SHELL=/bin/sh`` to ``SHELL=bin/bash``
#. Make slurm user and group

    .. code-block:: bash

       sudo groupadd -g 515 slurm
       sudo useradd -u 515 -g 515 slurm

#. Make biokem user and group:

    .. code-block:: bash

      sudo groupadd -g 2004664 biokempgrp
      sudo useradd -u 2004664 -g 2004664 biokem
      sudo mkdir /home/biokem
      sudo chown -R biokem /home/biokem
      sudo su biokem
      cd
      cp ../ubuntu/.profile .
      cp ../ubuntu/.bashrc .
      source .profile
      mkdir .ssh
      cd .ssh
      touch authorized_keys

#. Copy over curc.pub key
#. Update ``/projects/biokem/software/biokem/users/src/lab_specific/cryosparc_vms.src``

.. _Mount PL:

Mount lab PetaLibrary
^^^^^^^^^^^^^^^^^^^^^
Now we need to mount the lab's PetaLibrary to the VM, according to CURC's
`instructions <https://curc.readthedocs.io/en/latest/tutorials/cumulus4.html>`_.

#. Set up directories

    .. code-block:: bash

      exit
      sudo apt-get install sshfs
      sudo mkdir -p /pl/active/<lab's PL>
      sudo mkdir -p /pl/active/BioKEM/software/cryosparc/<lab>
      sudo chmod -R o+w /pl

#. Make key pair on VM

    .. code-block:: bash

      ssh-keygen -t ed25519

#. Add key to biokem on RC
#. Mount directories through fstab

    .. code-block:: bash

      #User lab PL
      biokem@dtn.rc.int.colorado.edu:/pl/active/aaronwhiteleylab /pl/active/aaronwhiteleylab fuse.sshfs defaults,_netdev,allow_other,default_permissions,identityfile=/home/ubuntu/.ssh/cryo,uid=biokem,gid=biokempgrp 0 0
      #User lab cryosparc worker
      biokem@dtn.rc.int.colorado.edu:/pl/active/BioKEM/software/cryosparc/aaronwhiteley /pl/active/BioKEM/software/cryosparc/aaronwhiteley fuse.sshfs defaults,_netdev,allow_other,default_permissions,identityfile=/home/ubuntu/.ssh/cryo,uid=biokem,gid=biokempgrp 0 0

#. If you want to mount manually:

    .. code-block:: bash

      sudo sshfs -o allow_other,IdentityFile=/home/ubuntu/.ssh/cryo biokem@dtn.rc.int.colorado.edu:/pl/active/<lab> /pl/active/<lab>
      sudo sshfs -o allow_other,IdentityFile=/home/ubuntu/.ssh/cryo biokem@dtn.rc.int.colorado.edu:/pl/active/BioKEM/software/cryosparc/<lab> /pl/active/BioKEM/software/cryosparc/<lab>

.. _Cryomaster:

Install 'master' Cryosparc
^^^^^^^^^^^^^^^^^^^^^^^^^^
Install the 'master' Cryosparc on the VM use their `instructions <https://guide.cryosparc.com/setup-configuration-and-management/how-to-download-install-and-configure/downloading-and-installing-cryosparc>`_.
But we need to make a few important changes for this to work.

#.Bring in presets

    .. code-block:: bash

      sudo su biokem
      cd
      git clone https://github.com/CU-BioKEM/cryosparc_setup.git
      cd cryosparc_setup
      nano license.src -> export LICENSE_ID=" "
      mkdir ~/cryosparc
      cd ~/cryosparc

#. Follow `instructions <https://guide.cryosparc.com/setup-configuration-and-management/how-to-download-install-and-configure/downloading-and-installing-cryosparc>`_

    .. code-block:: bash

      source ../cryosparc_setup/license.src
      curl -L https://get.cryosparc.com/download/master-latest/$LICENSE_ID -o cryosparc_master.tar.gz
      tar -xf *gz
      cd ../cryosparc_setup

#. Edit ``run_installer.sh`` and run
#. Edit ``ip_address.sh`` to correct IP and run
#. Start cryosparc

    .. code-block:: bash

      source ~/.bashrc
      cryosparcm restart

#. Connect cluster

    .. code-block:: bash

      cd alpine
      nano cluster_info.json -> edit to correct worker bin path
      cryosparcm cluster connect

#. Edit ``run_first_user.sh`` and run

.. _Cryoworker:

Install 'worker' Cryosparc
^^^^^^^^^^^^^^^^^^^^^^^^^^
Now that we've installed the 'master' instance, we can install the worker on Alpine.

#. Log onto RC

    .. code-block:: bash

      ssh login10
      cd /pl/active/BioKEM/software/cryosparc

#. Make a new directory for each lab

    .. code-block:: bash

      sudo -u biokem mkdir <labname>
      cd <labname>

    .. code-block:: bash

      git clone https://github.com/CU-BioKEM/cryosparc_setup.git
      cd cryosparc_setup

#. Edit license.src to add correct CryoSPARC license

    .. code-block:: bash

      nano license.src

    .. code-block:: bash

      cd ..
      source cryosparc_setup/license.src
      curl -L https://get.cryosparc.com/download/worker-latest/$LICENSE_ID -o cryosparc_worker.tar.gz
      tar -xf *gz

    .. code-block:: bash

      ssh login10
      ml slurm/alpine
      ainteractive
      ml cuda/11.4
      cd cryosparc_setup

#. Edit ``run_worker_install.sh``

    .. code-block:: bash

      ./run_worker_install.sh
      echo "export CRYOSPARC_SSD_PATH=\$SLURM_SCRATCH" >> ../cryosparc_worker/config.sh

#. Open new terminal

    .. code-block:: bash

      cryosparc

    Login and try to test it out. **Make sure you make all projects in PL**

.. _Cryo aliases:

Create CURC aliases
^^^^^^^^^^^^^^^^^^^
To keep everything as simple for the end user as possible, I have made lab
specific aliases in ``/projects/biokem/software/biokem/users/src/lab_specific``.
These will give users from each labs access to their specific Cryosparc builds.

#. Edit cryosparc_vms.src to add easy access to VM ``alias <lab>-cryosparc-vm="ssh -o KexAlgorithms=ecdh-sha2-nistp521 ubuntu@<IP>"`` (only gives access to BioKEM IT)
#. ``mkdir /projects/biokem/software/biokem/users/src/lab_specific/<lab>``
#. Update ``/projects/biokem/software/biokem/users/src/lab_specific/labs.src`` with new lab group
#. Make lab specific functions: ``touch <lab>lab.src``

     .. code-block:: bash

        #cryosparc
        alias cryosparc='export SLURM_CONF=/curc/slurm/alpine/etc/slurm.conf ;
                 echo -n "export " > ~/.slurm_token ;
                 scontrol token lifespan=86400 >> ~/.slurm_token ;
                 echo "export SLURM_CONF=/etc/slurm/slurm.conf" >> ~/.slurm_token ;
                 scp -o KexAlgorithms=ecdh-sha2-nistp521 ~/.slurm_token <admin>@<IP>:~/cryosparc_setup/export_tok$
                 firefox http://<IP>:<base port>'

#. Make admin functions

     .. code-block:: bash

        for USER in $(users)
          do
          if [ "$USER" == "<admin>" ]; then
            alias cryosparcm='ssh -o KexAlgorithms=ecdh-sha2-nistp521 <user>@<ip> "/home/<user>/cryosparc/cryosparc_master/bin/cryosparcm ${1}"'
            export PATH=/projects/biokem/software/biokem/users/src/lab_specific/luger:"$PATH"
          fi
          done`

#. Make ``cryosparc-add-key`` executable

     .. code-block:: bash

        !#/bin/bash

        IP=<IP>
        USER=biokem
        cat ${1} | ssh -o KexAlgorithms=ecdh-sha2-nistp521 ${USER}@${IP} 'cat >> .ssh/authorized_keys'
