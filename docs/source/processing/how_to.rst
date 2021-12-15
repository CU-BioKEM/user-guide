How to use blanca-biokem
========================

Getting started
---------------
In order to run software on BioKEM's Blanca servers:
   - Request access to Blanca through `Research Computing <https://rcamp.rc.colorado.edu/accounts/account-request/create/organization>`_
   - Request a Petalibrary allocation (~20TB/active project) by emailing rc-help@colorado.edu **Make sure to have them create a 'biokem-deposit' folder for your lab to deposit Krios images into**
   - Purchase an SBGrid license for your lab

     - Labs without an SBgrid license will have minimal access to software.
     - Labs with their own SBgrid license will be able to access the full stack of software, without limitation.


CCET users
----------

The Center for Cryo Electron Tomography (CCET) owns one of the nodes in the blanca-biokem cluster. Users from CCET can access this and other nodes in the blanca-biokem 
and blanca clusters by obtaining a guest account and following the general instructions.

Acquiring a guest account
-------------------------

To acquire a guest account you will need to go through the `Sponsored Affiliates <https://oit.colorado.edu/accounts/sponsored-affiliates>`_ processes.
After you acquire an account, have your CCET contact send an email to rc-help@colorado.edu to get added to the CCET group on Blanca. 

Using software
--------------

*You may need to submit a help ticket to gain access to EnginFrame.*

#. Log onto onto CU's VPN (`setup through OIT <https://oit.colorado.edu/vpn-virtual-private-network>`_)
#. Access blanca through the `EnginFrame <https://viz.rc.colorado.edu/enginframe/demo/index.html>`_ portal.
#. Click on ``Views``
#. Login with your identikey
#. Accept DUO Mobile push on your phone
#. Start a ``Remote Desktop`` session
#. Selected the desired number of CPUs (2 is usually fine), click ``Launch Session``
#. (may need to click on the thumbnail) Once in the remote desktop, click on the terminal icon on the menu bar
#. You are now on the viz node, which will be fine for applications like ChimeraX, but most of the time you'll want to be on a login node.
#. To get to a login node ``ssh -Y login10`` (can use 10-13, but this ssh requires a specific node number)
#. Enter password and accept DUO Mobile push
#. Now you can load the blanca envirnoment ``ml slurm/blanca``
#. The first time you log into blanca:
   
   - ``echo 'export MODULEPATH=$MODULEPATH:/projects/biokem/modules' >> ~/.bashrc``
   - ``source ~/.bashrc``

#. You should now be able to run ``ml avail`` and see a few new modules
#. To see all the modules available, you need to log onto a compute node interactively: ``sinteractive --qos=blanca-biokem --account=blanca-biokem --partition=blanca-biokem --ntasks=1``
#. From here you can troubleshoot software (you may need to request more resources)
#. To actually run software submit an sbatch job (RELION and CryoSPARC have GUIs that allow you to do this interactively)
