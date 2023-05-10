File transfer
=============

Transfer from camera
~~~~~~~~~~~~~~~~~~~~


Transfer to Globus
~~~~~~~~~~~~~~~~~~
We have a Globus endpoint configured on the storage server. This should allow
you to transfer images through CU's Globus to wherever.

#. Logon to the biokem-storage server.
#. Start screen for running client in:

    .. code-block:: bash

      screen -S globus

#. Log into your globus client:

    .. code-block:: bash

      globus login

#. Copy and paste hyperlink into web browser.
#. Code will be generated.
#. Copy and paste code into biokem-storage terminal.
#. Start the Globus connect:

    .. code-block:: bash

      ./home/biokem_manager/software/globus/globusconnectpersonal-3.1.5/globusconnectpersonal -start &

#. Detach from the screen:

    .. code-block:: bash

     ctrl+AD

#. Then, start the transfer on the Globus Connect GUI.
#. Kill screen when done


Transfer to AWS
~~~~~~~~~~~~~~~

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
