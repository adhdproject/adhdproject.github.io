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

And run the application (In this example we will monitor the www-data user).

`/opt/human.py$` **`python ./human.py`**
    
        Please run only as root
        I want good privilage separation with these log files
To run in root, just simply:

`/opt/human.py$` **`sudo su`**

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

`/opt/human.py#` **`python ./human.py www-data`**

        Starting mon service
        NO ALERT SERVICE ATTACHED
        ALERTS WILL BE PIPED TO STDOUT

The quasi error we can see in the output simply tells us that there is no
dedicated alert service attached.  As such, alerts will appear in the output
of the command.  If we want to, we can enable dedicated alerts via email by editing
human.py and setting the proper variables.

At this point, if a human makes an error while using the irc account, an alert will 
appear in our output.
        Alert <www-data> is acting like a human.
        
Example 2: Cancelling monitoring and purging records.
-----------------------------------------------------

human.py creates a log file for all activity on a monitored account.  By default
it is expected that you will only ever run human.py as root.  The file permissions
are set to only allow the user access to this sensitive file.

Human.py also sets the targeted account's bashrc to configure the account's shell 
to output errors to this log file.

Both of these two changes are reversed when you cancel monitoring.

To cancel monitoring, simply run the script in another terminal with the account name as the first argument and the word "stop" as the second.

`/opt/human.py#` **`python ./human.py www-data stop`**

        User Monitoring already configured
        Proceeding with monitoring.
        Ending monitoring of User.
        
This will delete the log file of the account. Because of this, the terminal that was running human.py will repeatedly output that the file is missing. Just Ctrl-c to stop it.

        cat: /var/log/human/www-data: No such file or directory
