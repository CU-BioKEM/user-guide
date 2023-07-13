File cleanup
============

There three files in the bin directory (``disk-use-check.sh``, ``remove-old-files.sh``, ``remove-old-dirs.sh``)
that are executed using a crontab entry:

    .. code-block:: bash
      
      0 7 * * * /home/biokem_manager/scripts/facility/bin/disk-use-check.sh
      0 5 * * * /home/biokem_manager/scripts/facility/bin/remove-old-files.sh
      0 * * * * /home/biokem_manager/scripts/facility/bin/remove-old-dirs.sh

- ``disk-use-check.sh`` emails a list of disk usage daily.
- ``remove-old-files.sh`` deletes files older than a certian number of days from the falcon server, interim_storage, and vault. Certain folders are protected and number of days can be edited, if desired.
- ``remove-old-dirs.sh`` does the same thing, but removes the empty directories left behind by the files script. As files nest down, it needs to run more fequently to remove all empty folders each day.