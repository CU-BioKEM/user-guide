Logging on
==========

**The steps below assume you have already completed the** :doc:`getting_started`
**steps.**

#. Log into `CURC Open OnDemand <https://ondemand.rc.colorado.edu>`_.

   - Use your CU Research Computing credentials and Duo 2-factor authentication to log in.
   - Read more about CURC Open OnDemand `here <https://curc.readthedocs.io/en/latest/gateways/OnDemand.html>`_

#. Click on ``My Interactive Sessions`` along the top.

#. To spawn a new remote desktop click ``Core Desktop`` in the ``Interactive Apps`` section along the left side of the page.

   - You can leave the ``Account`` blank
   - Fill in the desired number of hours you want the desktop job to persist (you can log back into it later, if the time requested isn't up), the default is generally fine.
   - Unless you plan to do computing on the viz node itself (which is allowed, but generally not necessary), you can leave the default number of cores
   - Click ``Launch``

#. You will be brought to a menu showing all current and recently completed remote desktops you have. Once the one you just opened it ready the header will turn green and a ``Launch Core Desktop`` button will appear, click it.

#. You are now on a remote desktop on one of the viz nodes. These nodes are equipped with 2 Quadro RTX 8000 GPUs to render graphics, although you may run compute jobs here, if you'd like.

#. Click the small black terminal icon on the top bar to open a terminal.

#. To run compute jobs follow the :doc:`using_software` guide.
