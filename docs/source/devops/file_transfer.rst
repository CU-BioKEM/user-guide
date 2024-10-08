File transfer
=============

Transfer from camera
~~~~~~~~~~~~~~~~~~~~
This script will transfer images from the falcon server to
the biokem-storage server as they are captured. It will apply
proper permissions for labs to access their files in the
interim folder. This script will also push images to a PetaLibrary
deposit folder, if specified. Should be run in a screen. It will
also place the newest gain ref into that folder.

    .. code-block:: bash

      Usage:
        /home/biokem_manager/scripts/facility/bin/transfer [options]
            -h,   (help)    show help message
            -s,   (source)  folder on falcon camera to be transfered
            -i    (interim) name of lab to transfer to in interim folder (i.e. luger-lab)
            -p    (pl)      optional flag to push to petalibrary (i.e. luger-lab)
            -a    (amazon)  optional flag to push to AWS bucket (i.e. ex1001)
            -g    (globus)  optional flag to push to shareable globus collection through the BioKEM PetaLibrary (i.e. ex1001)
            -d    (dropbox) optional flag to push to dropbox (i.e. ex1001)
            -b    (box)     optional flag to push to box (i.e. ex1001)
            -l    (longterm_storage) optional flag to push to /data/longterm_storage (i.e. luger-lab)
            -e    (email)   optional flag to send email completion email to recipient(s) (i.e. shla9937@colorado.edu)

To use:

#. Start screen on biokem-storage server.

    .. code-block:: bash
      
      screen -S transfer (or other name)

#. Start transfer 

    .. code-block:: bash

      transfer -s <folder> -i <lab name or "external"> <optional flags>

#. Detach from screen

    .. code-block:: bash  
      
      ctrl+ad

#. The screen running the transfer will terminate itself after 1 hour of being idle. 


Transfer to Globus
~~~~~~~~~~~~~~~~~~
We can use CURC's Globus endpoint to share data from BioKEM's PL. This should allow
you to transfer images through CU's Globus to wherever.

#. Logon to the biokem-storage server.
#. Start transfer with the ``-g`` flag (this will sync another copy of data to the ``/pl/active/BioKEM/biokem-deposit/globus`` folder on PL) 

    .. code-block:: bash

      transfer -s <folder> -i <lab name or "external"> -g <optional flags>

#. Detach from screen

    .. code-block:: bash  
      
      ctrl+ad

#. Logon to `Globus <https://www.globus.org/>`_
#. Search ``CU Boulder Research Computing`` in the file manager
#. Find the new or existing folder in ``/pl/active/BioKEM/biokem-deposit/globus/``
#. Click ``Share``
#. ``Add Guest Collection``
#. Add a ``Display Name``
#. ``Create Collection``
#. ``Add Permissions - Share With``
#. Make sure ``Path`` is ``/`` and add email (only need read permission)
#. The users will see data as the transfer script deposits it. 


Transfer to AWS
~~~~~~~~~~~~~~~
AWS credentials are stored at ``/home/biokem_manager/scripts/facility/bin/s3_configs``.
A new one needs to be configured for every new users. AWS transfers are handled by the ``transfer`` script:

    .. code-block:: bash

      transfer -s <folder> -i external -a <ex1001 or other>

We are using the ``s5cmd`` to transfer data in parallel. Each new AWS needs a new ``_v5.cfg`` file and an entry in the ``buckets.txt`` file. 

Transfer to networked server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
When users are created on the storage server, their ``interim_storage`` will be
configured to give only that lab access to their data. This way, they will only
be able to copy their own data off the server and no one else should be able to
see it. And example command, to be run on the user's server can be found in
:doc:`getting` External customers do not have access to the server, their data will
be controlled by the facility manager.


Transfer to HDD
~~~~~~~~~~~~~~~
