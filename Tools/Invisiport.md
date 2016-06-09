
Invisiport
=======

Website
-------

<https://bitbucket.org/Zaeyx/invisiport>

Description
-----------

Invisiport is an evolution to the honeyports concept.  With honeyports,
it is decently obvious when you trigger the defenses, as the host you
scanned will drop away.  (That is, it will start refusing connections from you).
This can lead an attacker to simply bypass the blacklist by changing his IP address.

But what if the attacker didn't know he had been blacklisted?  In that case he
is less likely to even consider circumventing our defenses!  And as long as he actually
is blocked, we're far more safe than before.

Invisiport accomplishes this by having a few different ports it listens on.
First it has a trigger port.  This is the port that will trigger the block.
Just like with a traditional honeyport.  Next it has a false portset.  This is a
list of ports that invisiport will spoof listening on, to any clients that trigger a block.

That is, that anyone who connects to the listen port will be blacklisted.  But from
their perspective, if they scan the host again, they will see the listen port, and the 
false ports as still open and listening.  That way, unless they're quite clever 
they are unlikely to figure out that they have been blacklisted.

Install Location
----------------

`/opt/invisiport/`

Usage
-----


Launching invisiport is very easy.  Just cd into the directory.

`~$` **`cd /opt/invisiport`**

And run the application

`/opt/invisiport$` **`sudo python ./invisiport.py`**

You shouldn't see any output.  But the terminal should hang.  That's okay.
The script is running in the background with the default configurations.

At this point it is fully functional, and working to protect you.

Next we will cover how to customize the configurations.

Example 1: Customizing the Configurations
-----------------------------------------

The configurations for invisiport are very easy to set.  Simply
edit the variables at the front of the script to set their respective components.

You can set the whitelist by editing the "whitelist" variable.
It is in a python list format.  Just add another address in this format
to add to the whitelist.  Any address on the whitelist cannot be blacklisted.

Next is the python list "ports" these are the ports that will be shown to
a blacklisted host as still open and available.  You can set them to mimic whatever
you like.  By default they mimic an ftp, http, and smb server.

The PORT variable is a simple integer variable.  This sets the trigger/listen port
that will blacklist those connecting to it.

You can also configure the blacklist variable to change the blacklist file.  This file
records all blacklisted hosts.

