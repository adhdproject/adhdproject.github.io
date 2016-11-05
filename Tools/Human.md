Human.py
=======

Website
-------

<https://bitbucket.org/Zaeyx/human.py>

Description
-----------

Human.py (Aka human pie) is a script made for the sole purpose of detecting human usage of 
service accounts.  You can set it up to watch accounts you suspect may be attacked.
The assumption is that the account is only used by a service (or services) and should not be 
accessed by a human user.  If a human user does however manage to compromise the account
this script can detect human activity by watching for indicators of such (like mistyped commands).

Install Location
----------------

`/opt/human.py/`

Usage
-----


Running Human.py couldn't be easier, simply cd to the correct directory.

`~$` **`cd /opt/human.py`**

When we try to just run the application we see that it needs to be run as root.

`/opt/human.py$` **`python ./human.py`**
    
        Please run only as root
        I want good privilage separation with these log files
To run as root, just simply:

`/opt/human.py$` **`sudo python ./human.py`**

This will show you the very very simple help dialog.

        Human identification on service accounts
        Proper Usage
        ./human.py <username_to_monitor>
        or
        ./human.py <username_to_stop_monitoring> stop

Example 1: Setting up Monitoring on a service account
-----------------------------------------------------

To set up monitoring on a service account run the tool with the name of the 
account as the first command line argument.

NOTE: For this example I used the postgres account as my target.  You may or may not have this account on your machine.  Feel free to just use your personal user as the target. 

`/opt/human.py#` **`python ./human.py postgres`**

        Starting mon service
        NO ALERT SERVICE ATTACHED
        ALERTS WILL BE PIPED TO STDOUT

The quasi error we can see in the output simply tells us that there is no
dedicated alert service attached.  As such, alerts will appear in the output
of the command.  If we want to, we can enable dedicated alerts via email by editing
human.py and setting the proper variables.

At this point, if a human makes an error while using the monitored account, an alert will 
appear in our output.
        Alert <postgres> is acting like a human.

NOTE: This tool can only be used on accounts to whom the right to run the bash shell is given.  To see which accounts on your machine can run with the bash shell run the following command **`grep bash /etc/passwd`**
        
Example 2: Cancelling monitoring and purging records.
-----------------------------------------------------

human.py creates a log file for all activity on a monitored account.  By default
it is expected that you will only ever run human.py as root.  The file permissions
are set to only allow the user access to this sensitive file.

Human.py also sets the targeted account's bashrc to configure the account's shell 
to output errors to this log file.

Both of these two changes are reversed when you cancel monitoring.

To cancel monitoring, simply run the script in another terminal with the account name as the first argument and the word "stop" as the second.

`/opt/human.py#` **`python ./human.py postgres stop`**

        User Monitoring already configured
        Proceeding with monitoring.
        Ending monitoring of User.
        
This will delete the log file of the account. Because of this, the terminal that was running human.py will repeatedly output that the file is missing. Just Ctrl-c to stop it.

        cat: /var/log/human/postgres: No such file or directory
