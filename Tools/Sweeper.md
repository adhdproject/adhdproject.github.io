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

For this example, we will use 86, 75, and 309.  

`/opt/sweeper$` **`python ./sweeper.py 86 75 309`**

On another machine, try to netcat to the ADHD machine. Say the IP address of the ADHD machine is 192.168.56.101 and the IP address of the other machine is 192.168.56.1:

`$` **`nc 192.168.56.101 86`**

The output on the ADHD machine will look like this:

        Connection ('192.168.56.1', 49884)->86

Do this a couple more times with the same port or one of the other two ports. The ADHD machine will stop connections entirely and output this:

        Actions taken against 192.168.56.1
        IP blocked via Iptables
        
You can try again by flushting the IP tables.

`/opt/sweeper$` **`sudo iptables -F`**

And there you have it.
