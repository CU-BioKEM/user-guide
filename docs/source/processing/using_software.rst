Using software
==============

Notes: 
* The steps below assume you have already completed the [Getting Started](./getting_started.html) steps, including [setting up a CU Research Computing account](https://www.colorado.edu/rc) using your CU Boulder IdentiKey.

### Step 1: Log onto onto CU's VPN (`setup through OIT <https://oit.colorado.edu/vpn-virtual-private-network>`_)

### Step 2: Start a Remote Desktop session on CURC Open OnDemand

[CURC Open OnDemand](https://curc.readthedocs.io/en/latest/gateways/OnDemand.html) is a browser based, integrated, single access point for all of your high performance computing (HPC) resources at CU Research Computing. CURC Open OnDemand provides a graphical interface to view, edit, download, and upload files, manage and create job templates for CURC's clusters, and access CURC interactive applications (Virtual Desktops, Matlab, and JupyterNotebooks), all via a web browser and with minimal knowledge of Linux and scheduler commands required.

#### Login to CURC Open OnDemand
 
To connect to CURC Open OnDemand, navigate to [https://ondemand.rc.colorado.edu](https://ondemand.rc.colorado.edu/). The first page of CURC Open OnDemand will bring you to a login prompt. Use your CU Research Computing credentials and Duo 2-factor authentication to login. If you need a CU Research Computing account please visit [our account request page](https://curc.readthedocs.io/en/latest/access/logging-in.html) to get started.

![](OnDemand/login_page.png)

_**Notes on logging out:**_ 
* You must **completely quit your browser in order for "logout" to occur**. If after reopening your browser you are still logged in, please clear your cookies. If you are on a Chromebook or Chromebox, you will need to reboot your device in order to "quit the browser" and thus "logout".  
* **Tip: Using a "private browsing mode"** window while using OnDemand is a great way to handle "auto-logout", as closing your browser window will remove all associated cookies and session information. 

### Features

When you have successfully logged into CU Open OnDemand you will see the landing page with the following features broken into tabs along the top of the page: __Files__, __Jobs__, __Clusters__ (shell access), __Interactive Apps__, and __My Interactive Sessions__.

![](OnDemand/landing_page.png)

We'll step through these features one at a time.

#### Files

The _File_ menu allows you to view and operate on files in different file spaces: your _home_ directory, _projects_ directory, scratch spaces, and (if you own one or are part of a group with one) access to _PetaLibary_ allocations.

Selecting one of the file spaces will open a separate browser window which will allow you to navigate the space in a graphical setting similar to a file browser on a personal computer. From here you can download, upload, create, delete, and open files.

> **_NOTE:_** _Please use [Globus](https://curc.readthedocs.io/en/latest/compute/data-transfer.html#globus-transfers) to transfer files to/from `/scratch/summit` or `/rc_scratch` or if you have more than 1 GB of data to transfer. OnDemand is not designed to handle large file transfers._

![](OnDemand/home_file.png)

> For additional documentation visit [OSC's File Transfer and Management help page](https://www.osc.edu/resources/online_portals/ondemand/file_transfer_and_management).

#### Jobs 

Jobs can be monitored, created, edited and scheduled with the job management tools under the _Jobs_ menu.

* __Active Jobs:__ Under the “Active Jobs” tab you can view active jobs. You can choose to view your jobs (or all jobs) as well as choose from specific clusters (Alpine, Core, Summit, or All Clusters). From this menu you can cancel your own jobs.

![](OnDemand/active_jobs.png)

* __Job Composer:__ Create and edit job scripts and schedule jobs under this menu.

> See [OSC's Job Management help page](https://www.osc.edu/resources/online_portals/ondemand/job_management) for detailed use of the job composer feature.

![](OnDemand/job_composer.png)

#### Clusters (Shell Access)

The _Clusters_ menu provides shell access to login nodes on CURC clusters. The shell terminal is similar to many other tools that provide terminal access.

> Currently both Summit and Alpine shells log you into a basic login node, you can load in either Alpine, Summit, or Blanca slurm instances from here: `module load slurm/<cluster>` with either summit, alpine, or blanca.

* __Alpine:__ The Alpine tab will launch a terminal that RC users can use to manually access an RC Login node. After the tab opens, type your CURC password and accept the Duo push to your phone to complete login to the terminal.

* __Summit:__ The Summit tab will launch a terminal that RC users can use to manually access an RC Login node. After the tab opens, type your CURC password and accept the Duo push to your phone to complete login to the terminal.

<!-- * __Blanca:__ The Blanca tab will launch a terminal that RC users can use to manually access an RC Login node. After the tab opens, type your CURC password and accept the Duo push to your phone to complete login to the terminal. To load the Blanca Slurm environment, make sure to type “module load slurm/blanca upon login. More information on using the Blanca cluster can be found [in the Blanca documentation](https://curc.readthedocs.io/en/latest/access/blanca.html). -->

#### Interactive Applications Menu

The _Interactive Applications_ menu contains options to launch certain applications that have graphical user interfaces (GUIs), for interactive use on CURC clusters. Current supported applications include a __remote desktop atop the core cluster__, __MATLAB__, and __JupyterHub__ .

##### Core Desktop (Remote Desktop)

1. When starting a Remote Desktop session you may customize the resources allocated to the session and other characteristics of the dispatched Slurm job. The default will open a Remote Desktop with 1 node (2 cores) for 1 hour (no need to specify account).
![](OnDemand/core_launch.png)
2. Click “launch” to submit the Remote Destop job to the queue. The wait time depends on the number of other users presently on the resource. Requesting smaller, shorter jobs may faciliate shorter wait times. 
3. When your Remote Desktop is ready you can click the "Launch Core Desktop" button. In most cases the default compression and image quality will suffice but if you do have problems with image quality you can adjust these settings as necessary. 
<!-- ![](OnDemand/core_session.png) -->
4. With the Remote Desktop session running and open, you should be able to run standard Linux desktop applications that have a graphical user interface (GUI). Instructions to use graphical software can be found at the [RC EnginFrame page](https://curc.readthedocs.io/en/latest/gateways/enginframe.html#step-3-use-graphical-software).

**Notes**:
* You can copy/paste into/out of the VNC desktop using the clipboard in the "hidden" tab on the left-hand-side of the virtual desktop.
![](OnDemand/copypasta.png)
* Closing the window will not terminate the job, you can use the “My Interactive Sessions” tab to view all open interactive sessions and terminate them.


#### My Interactive Sessions

The _My Interactive Sessions_ menu will let you view and manage all of your current open Interactive applications. From this window you can view the node/core count, status, as well as time remaining for current sessions. 

Closing the window an interactive application is opened in will not terminate the session, you will need to click the “Delete” button for the individual session.
![](OnDemand/my_interactive_sessions.png)


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
   
   - ``echo 'export MODULEPATH=/projects/biokem/modules:/programs/share/modulefiles/x86_64-linux:"$MODULEPATH"' >> ~/.bashrc``
   - ``source ~/.bashrc``

#. You should now be able to run ``ml avail`` and see a few new modules
#. To see all the modules available, you need to log onto a compute node interactively: ``sinteractive --qos=blanca-biokem --account=blanca-biokem --partition=blanca-biokem --ntasks=1``
#. From here you can troubleshoot software (you may need to request more resources)
#. To actually run software submit an sbatch job (RELION and CryoSPARC have GUIs that allow you to do this interactively)
