
Simple-Pivot-Detect
=======

Website
-------

<https://bitbucket.org/Zaeyx/simple-pivot-detect>

Description
-----------

Simple-pivot-detect is exactly what its name implies.  A script that 
requires next to no knowledge to be able to quickly detect pivot behavior
in action on your network.  Run it on a box that you think might be 
compromised and being used as a pivot.  If it finds anything, it will alert
you.

It works by checking for processes with network activity.  And then following
the parent process id to find if one of the processes' ancestors also has 
network activity.  So if the attacker you're tracking comes in from the network
and then spawns another process that goes out to another box, this script should
detect that and alert you.

Obviously there are ways around this.  But, most attackers aren't going to go 
to great lengths to hide the process trail that created a pivot.  It's decently 
rare for someone to be detected on the basis of their pivoting, mostly because 
scripts like this aren't in wide circulation.  

So, use simple-pivot-detect when you suspect a pivot.  At a minimum, you'll
force the attacker to spend more time and resources hiding from you.

Install Location
----------------

`/opt/simple-pivot-detect/`

Usage
-----


Launching simple-pivot-detect is super simple.  Just cd into the directory.

`~$` **`cd /opt/simple-pivot-detect`**

And run the script

`/opt/simple-pivot-detect$` **`python ./simple-pivot-detect.py`**

It's exactly that simple.  It will alert you if it finds anything suspicious
by feeding back to stdout the PID trail of the process(es) it is alerting on.