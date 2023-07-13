User maintenance 
================

Making a new user on the storage server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Simply use the ``make_new_user.py`` script that is in your path. 
This script will:

    - Create new user
    - Make folder of same name in the ``interim_storage``  
    - Set proper permissions on that folder

Removing a user on the storage server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Simply use the ``delete_old_user.py`` script that is in your path. 
This script will:

    - Remove the user account
    - Delete the user's ``interim_storage`` folder
    - Delete the user's ``home`` folder