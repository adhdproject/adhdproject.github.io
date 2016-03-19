
Sweeper
=======

Website
-------

<https://bitbucket.org/Zaeyx/sweeper>

Description
-----------

Sweeper is an evolution of the honeyports concept.

Rather than simply blocking an IP address for a simple visit to a port.
Sweeper only blocks an IP for repeated connections, or for scanning activity.

In this way Sweeper is a much more conservative honeyport.

Install Location
----------------

`/opt/sweeper/`

Usage
-----


Launching sweeper is super simple.  Just cd into the directory.

`~$` **`cd /opt/sweeper`**

And run the script

`/opt/sweeper$` **`python ./sweeper.py`**

The help dialog will be displayed.

		Improper Usage
		More like this...
		sweeper.py <port1> <port2> <port3>

Example 1: Setting a Trap
-----------------------------------------

For this example, let's create a simple trap with Sweeper.

Sweeper requires that we specify three different ports to listen on.
The ports we choose are up to us.  But we will want to think about the
ports that we choose.  Sweeper basically keeps a "score" for each IP address
that connects to it.  Every time the IP connects again it increments the score
by one.  If the score for a certain IP passes a threshold specified in the 
script's configuration, that IP is then blocked.

So hosts can be blocked for visiting the same port repeatedly.  Or for scanning
activity that visits more than one of sweeper's listening ports.

As such, choose your ports wisely, based on your unique situation.

For this example, we will use 21, 139, and 445.  

`/opt/rubberglue` **`python ./sweeper.py 21 139 445`**

That's it.  


