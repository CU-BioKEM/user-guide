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

  #. run the command ``cryosparc add-key <new users public RSA key>``
  #. Add them in the GUI as normal

**The first step is necessary to allow the user to generate new tokens that needs
Cryosparc to submit jobs to SLURM. Without doing this, they will only be able to
submit jobs within 24hrs of someone who has token access logging onto CURC**

.. _List of ports:

List of ports
-------------
To access Cryosparc port forwarding is necessary. To avoid getting the wrong
forward, each lab will have their own set of ports listed below (only affects
the CURC side of things, not the VM). You do not need to do anything with these,
this is for IT housekeeping purposes only.

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

#.

.. _SLURM integration:

Integrate SLURM
^^^^^^^^^^^^^^^

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
