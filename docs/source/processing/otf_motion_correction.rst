On-the-fly motion correction
============================
The BioKEM facility can also offer on-the-fly motion correction and 2D classification 
through CryoSPARC or RELION (if you have a valid license) using our dedicated 
preprossessing node. Ask Chuck for more details.

CryoSPARC Live
--------------

.. image:: ../images/schematic_cryosparclive.png
   :width: 100
   :align: left

To use CryoSPARC Live through the BioKEM facility we will employ a virtual 
machine hosted on CUmulus. This vitrual machine will allow us to run jobs 
longer than normally allowed and still be able to submit processing tasks 
through slurm to blanca. 

RelionOTF
---------
RELION on-the-fly can be ran by the facility by running an interactive job on 
the preprocessing node. The ``Schedules`` feature of RELION allows it to detect
new images as they are pushed to petalibrary, much the same was as CryoSPARC Live,
but with less feedback.
