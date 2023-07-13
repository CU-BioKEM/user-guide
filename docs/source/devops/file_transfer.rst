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

Usage:
  /home/biokem_manager/scripts/facility/bin/transfer [options]
      -h,   (help)    show help message
      -s,   (source)  folder on falcon camera to be transfered
      -i    (interim) name of lab to transfer to in interim folder (i.e. luger-lab)
      -p    (pl)      optional flag to push data to petalibrary (i.e. luger-lab)
      -a    (amazon)  push to AWS bucket (i.e. ex1001)
      -g    (globus)  stage copy of folder in shareable globus collection 

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
We have a Globus endpoint configured on the storage server. This should allow
you to transfer images through CU's Globus to wherever.

#. Logon to the biokem-storage server.
#. Start globus endpoint (this is a custom script, not a built in function)

    .. code-block:: bash
      
      globus-start

#. Start screen on biokem-storage server.

    .. code-block:: bash
      
      screen -S transfer (or other name)

#. Start transfer with the ``-g`` flag (this will sync another copy of data to the ``/data/globu_transfers`` folder) 

    .. code-block:: bash

      transfer -s <folder> -i <lab name or "external"> -g <optional flags>

#. Detach from screen

    .. code-block:: bash  
      
      ctrl+ad

#. Logon to `Globus <https://www.globus.org/>`_
#. If guest collection is already set up:

  #. Search ``biokem-guest``
  #. ``Permissions``
  #. ``Add Permissions``
  #. Make sure path is ``/`` and add email

#. If guest collection is not already set up:

  #. Search collection for ``biokem-storage``
  #. Click ``share``
  #. ``Add Guest Collection``
  #. Fill out and ``Create Guest Collection``
  #. ``Add Permissions`` 
  #. Put in their email and ``Add Permission``

#. The users will see data as the transfer script deposits it. 
#. Once the user has gotten their data:

  #. Revoke access to the endpoint using the ``Permissions`` page
  #. End the globus enpoint 

      .. code-block:: bash

        globus-stop


Transfer to AWS
~~~~~~~~~~~~~~~
AWS credentials are stored at ``/home/biokem_manager/scripts/facility/bin/s3_configs``.
A new one needs to be configured for every new users. AWS transfers are handled by the ``transfer`` script:

    .. code-block:: bash

      transfer -s <folder> -i external -a <ex1001 or other>

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
