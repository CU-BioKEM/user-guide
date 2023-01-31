Cryosparc
=========

.. _General users:

General users
-------------
Once your lab has set up a Cryosparc installation with the BioKEM IT person, all
you need to do to run it is:

  #. Log on to OpenOnDemand (:doc:`../logging_on`)
  #. Run the command ``cryosparc``

.. _Lab admins:

Lab admins
----------
To add new users:

  #. Have the user send their curc.pub key

     - Let's store these in lab PL or projects (you do not want to swap this for your own key)
     - Have user run ``cp ~/.ssh/curc.pub <path to shared storage location>/$USER_curc.pub``

  #. Add their curc.pub key to the VM ``cat <user>_curc.pub | ssh -o KexAlgorithms=ecdh-sha2-nistp521 <use>r@<IP> 'cat >> .ssh/authorized_keys'``
  #. Add them in the GUI as normal

**The first two steps are necessary to allow the user to generate new tokens that needs
Cryosparc to submit jobs to SLURM. Without doing this, they will only be able to
submit jobs within 24hrs of someone who has token access logging onto CURC**

You should be able to run ``cryosparcm`` commands as normal, although I have not
 tested updating. You may have to go into the cryosparc_worker directory and
that manually.

.. _List of ports:

List of ports
-------------
To access Cryosparc port forwarding is necessary. To avoid getting the wrong
forward, each lab will have their own set of ports listed below. You do not need
 to do anything with these, this is for IT housekeeping purposes only.

  +-------------+------------------+
  | Ports       | Lab              |
  +-------------+------------------+
  | 39000-39009 | Luger            |
  +-------------+------------------+
  | 39010-39019 | Sousa            |
  +-------------+------------------+
  | 39020-39029 | Whiteley (Aaron) |
  +-------------+------------------+
  | 39030-39039 | Kasinath         |
  +-------------+------------------+
  | 39040-39049 | Aydin            |
  +-------------+------------------+
  | 39050-39059 | Cech             |
  +-------------+------------------+
  | 39060-39069 | Wuttke           |
  +-------------+------------------+
  | 39070-39079 | Taatjes          |
  +-------------+------------------+

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

#. On VM:

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

#. Log onto RC to find lab admin's user and group (BioKEM user in future):

    .. code-block:: bash

      id -u $USER
      id -g $USER
      whoami
      id -g -n $USER

#. On VM (make sure to clone correct slurm):

    .. code-block:: bash

      sudo groupadd -g <group num> <group name>
      sudo useradd -u <user num> -g <user num> <user>
      sudo mkdir /home/<user>
      sudo chown -R <user> /home/<user>
      sudo su <user>
      cd
      cp ../ubuntu/.profile .
      cp ../ubuntu/.bashrc .
      mkdir .ssh
      cd .ssh
      touch authorized_keys

#. Copy over curc.pub key ``cp ~/.ssh/curc.pub <path to shared storage location>/$USER_curc.pub``

.. _Mount PL:

Mount lab PetaLibrary
^^^^^^^^^^^^^^^^^^^^^
Now we need to mount the lab's PetaLibrary to the VM, according to CURC's
`instructions <https://curc.readthedocs.io/en/latest/tutorials/cumulus4.html>`_.

.. code-block:: bash

  exit (back to root user)
  sudo apt-get install sshfs
  sudo mkdir -p /pl/active/<lab's PL>
  sudo chmod -R o+w /pl
  sudo sshfs -o allow_other <user>@dtn.rc.int.colorado.edu:/pl/active/<lab> /pl/active/<lab>

.. _Cryomaster:

Install 'master' Cryosparc
^^^^^^^^^^^^^^^^^^^^^^^^^^
Install the 'master' Cryosparc on the VM use their `instructions <https://guide.cryosparc.com/setup-configuration-and-management/how-to-download-install-and-configure/downloading-and-installing-cryosparc>`_.
But we need to make a few important changes for this to work.

.. code-block:: bash

  sudo su <user>
  mkdir /pl/active/<lab>/cryosparc_projects
  chmod g+w -R /pl/active/<lab>/cryosparc_projects
  cd
  git clone https://github.com/CU-BioKEM/cryosparc_setup.git
  cd cryosparc_setup
  nano license.src -> export LICENSE_ID=" "
  mkdir ~/cryosparc
  cd ~/cryosparc

Follow `instructions <https://guide.cryosparc.com/setup-configuration-and-management/how-to-download-install-and-configure/downloading-and-installing-cryosparc>`_

.. code-block:: bash

  source ../cryosparc_setup/license.src
  curl -L https://get.cryosparc.com/download/master-latest/$LICENSE_ID -o cryosparc_master.tar.gz
  tar -xf *gz
  cd ../cryosparc_setup

Edit ``run_installer.sh`` and run
Edit ``ip_address.sh`` to correct IP and run
Edit ``run_first_user.sh`` and run

.. code-block:: bash

  source ~/.bashrc
  cryosparcm restart
  cd alpine
  cryosparcm cluster connect


.. _Cryoworker:

Install 'worker' Cryosparc
^^^^^^^^^^^^^^^^^^^^^^^^^^
Now that we've installed the 'master' instance, we can install the worker on Alpine.

Log onto RC

.. code-block:: bash

  git clone https://github.com/CU-BioKEM/cryosparc_setup.git
  source cryosparc_setup/license.src
  curl -L https://get.cryosparc.com/download/worker-latest/$LICENSE_ID -o cryosparc_worker.tar.gz
  tar -xf *gz

.. code-block:: bash

  ssh login10
  ml slurm/alpine
  ainteractive
  ml cuda/11.4
  cd cryosparc_setup
  ./run_worker_install.sh
  echo "export CRYOSPARC_SSD_PATH=\$SLURM_SCRATCH" >> ../cryosparc_worker/config.sh

Open new terminal

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
#. Update ``/projects/biokem/software/biokem/users/src/lab_specific/labs.src`` with new lab group
#. Make lab specific functions
  - ``touch <lab>lab.src``
  -   .. code-block:: bash

        #cryosparc
        alias cryosparc='export SLURM_CONF=/curc/slurm/alpine/etc/slurm.conf ;
                 echo -n "export " > ~/.slurm_token ;
                 scontrol token lifespan=86400 >> ~/.slurm_token ;
                 echo "export SLURM_CONF=/etc/slurm/slurm.conf" >> ~/.slurm_token ;
                 scp -o KexAlgorithms=ecdh-sha2-nistp521 ~/.slurm_token <admin>@<IP>:~/cryosparc_setup/export_tok$
                 firefox http://<IP>:<base port>'

#. Make admin functions
  - .. code-block:: bash

      for USER in $(users)
        do
        if [ "$USER" == "<admin>" ]; then
          alias cryosparcm='ssh -o KexAlgorithms=ecdh-sha2-nistp521 <user>@<ip> "/home/<user>/cryosparc/cryosparc_master/bin/cryosparcm ${1}"'
          alias cryosparc-add-key='cat ${1} | ssh -o KexAlgorithms=ecdh-sha2-nistp521 <user>@<ip> "cat >> .ssh/authorized_keys"'
        fi
        done
