Storing data
============

This guide will detail the storage options avaible to BioKEM users, as well as their 
limitations. 

General flow
------------
The BioKEM facility has a number of storage devices for different applications:

    - :ref:`Falcon server`
    - :ref:`Interim storage`
    - :ref:`Vault`

+----------------+--------+-------+----------+----------------------------+
| Device         | Size   | Type  | User     | Length of storage          |
+----------------+--------+-------+----------+----------------------------+
| Falcon Server  | 70TB   | RAID0 | Facility | 2 weeks (auto deleted)     |
+----------------+--------+-------+----------+----------------------------+
| Interim storage| 109TB  | RAID6 | User     | 2 weeks (auto deleted)     |
+----------------+--------+-------+----------+----------------------------+
| Vault          | 58TB   | RAID6 | External | 3 months                   |
+----------------+--------+-------+----------+----------------------------+

.. image:: images/schematic.png
   :width: 300
   :align: right

.. _Falcon server:

Falcon server
-------------
The Falcon server contains 70TB of RAID0 storage for storing images directly off the Krios. 
This is enough storage for roughly one month of data collection, however this
storage device is not redundant on its own, so data is pushed live to the 
:ref:`Interim storage` partition of the biokem-storage server and the PetaLibrary, if possible.
Data is automatically deleted after 2 weeks.

.. _Interim storage:

Interim storage
--------------
Interim storage is a costumer facing partition of the biokem-storage device. This is
where users can pull their data. Data is automatically deleted after 2 weeks. 

.. _Vault:

Vault
-----
The Vault is a separate RAID6 storage device within the biokem-storage server with
a capacity of 58TB. This partition is used to archive external users' data, if they
require a redundant backup. Storage on this device is limited to 3 months.   

