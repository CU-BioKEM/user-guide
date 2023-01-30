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
Due to the nature of integrating cryosparc into CURC's environment, updating
versions or doing many other administrative tasks will not be supported at this
time. However, you will be able to add new users. To do this:

  #. Have the user send their curc.pub key

     - Let's store these in lab PL or projects (you do not want to swap this for your own key)
     - Have user run ``cp ~/.ssh/curc.pub <path to shared storage location>/$USER_curc.pub``

  #. Add their curc.pub key to the VM ``cat <user>_curc.pub | ssh user@hostname 'cat >> .ssh/authorized_keys'
  #. Add them in the GUI as normal

**The first step is necessary to allow the user to generate new tokens that needs
Cryosparc to submit jobs to SLURM. Without doing this, they will only be able to
submit jobs within 24hrs of someone who has token access logging onto CURC**

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
  sudo scp <user>@login.rc.colorado.edu:/curc/slurm/alpine/etc/slurm.conf .
  sudo nano slurm.conf
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
    sudo useradd -u 569708 -g <user num> <user>
    sudo mkdir /home/<user>
    sudo chown -R <user> /home/<user>
    sudo su shla9937
    cd
    cp ../ubuntu/.profile .
    cp ../ubuntu/.bashrc .
    mkdir setup
    mkdir .ssh
    cd .ssh
    touch authorized_keys

#. Copy over curc.pub key ``cp ~/.ssh/curc.pub <path to shared storage location>/$USER_curc.pub``

add lab admin's RSA key

.. _Mount PL:

Mount lab PetaLibrary
^^^^^^^^^^^^^^^^^^^^^

.. _Cryomaster:

Install 'master' Cryosparc
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Cryoworker:

Install 'worker' Cryosparc
^^^^^^^^^^^^^^^^^^^^^^^^^^

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
                 scontrol token lifespan=86400 > ~/.slurm_token ;
                 echo -n "export SLURM_CONF=/etc/slurm/slurm.conf" >> ~/.slurm_token ;
                 scp -o KexAlgorithms=ecdh-sha2-nistp521 ~/.slurm_token <admin>@<IP>:~/cryosparc_setup/export_tok$
                 firefox http://<IP>:<base port>'
