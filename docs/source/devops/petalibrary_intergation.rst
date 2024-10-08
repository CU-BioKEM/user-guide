Petalibrary Intergation
=======================

In order to facilitate fast transfer of CU-Boulder user's data to their PL. The Biokem
service account on CURC infastructure has been added to each of the lab's who have PL's.

Within the base level of each of these labs is a biokem owned folder called `biokem-deposit`.
This folder allows us to transfer data without user input. If a new lab is added:

    #. Have CURC follow their own documentation on setting this up `here <https://curc.readthedocs.io/en/latest/additional-resources/biokem-facility.html>`_
    #. Make a folder using the `-lab` suffix in the `/mnt/pl/` folder on the storage server
    #. Create an `fstab` entry to point the new folder to the `biokem-deposit` folder in their PL
    #. Mount the folder (`sudo mount -av`)

This integration also allows for us to set up a `CryoSPARC <https://biokem-user-guide.readthedocs.io/en/latest/devops/cryosparc_setup.html>`_ VM for them.