Getting your data
=================

.. image:: images/schematic_networked.png
   :width: 300
   :align: right

Push to Petalibrary (*Preferred method*)
----------------------------------------
The preferred method of getting your data from the BioKEM facility is to have us push
it to your `biokem-deposit` folder. We can do this in real time so you can start 
processing data as it comes off the scope (we can also do 
:doc:/computing/otf_motion_correction for you). You can also push your data 
Petalibrary from :ref:`Interm storage` once Chuck has set you up with a user profile 
on the biokem-storage server.

Globus
------
The biokem-storage server has a Globus endpoint configured, which allows users
outside (or inside, if not better method exists) to transfer their data through
Globus. The is a fairly secure option, although can be slow.

#. Logon to the biokem-storage server.
#. Start screen for running client in:
  ``screen -S globus``
#. Log into your globus client:
   ``globus login``
#. Copy and paste hyperlink into web browser.
#. Code will be generated.
#. Copy and paste code into biokem-storage terminal.
#. Start the Globus connect: 
   ``./home/biokem_manager/software/globus/globusconnectpersonal-3.1.5/globusconnectpersonal -start &``
#. Detach from the screen:
   ``ctrl+AD``
#. Then, start the transfer on the Globus Connect GUI. 

AWS
---
If your lab/organization is user of AWS you may have your data dropped into an
AWS bucket. (We are still setting this up)

Sync to network server
----------------------
A secure and slightly slower alternative to Petalibrary is to transfer your data
to a server your lab owns on CU's network. Once Chuck sets you up with a user 
account on the biokem-storage server you will be able to use an ``rsync`` command
to transfer your data to your server from the ``Interm storage`` partition.

External hard drive (*Discouraged method*)
------------------------------------------
Transferring data via physically transporting an external hard drive is discouraged,
as these disks are non-redundant, slow, and prone to physical damage. If none of
the other methods are available to you however; you may drop off a hard drive to
transfer your data.
