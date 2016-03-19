
Artillery
=========

Website
-------

<https://www.trustedsec.com/downloads/artillery/>

Description
-----------

The purpose of Artillery is to provide a combination of honeypot, file-system
monitoring, system hardening, real-time threat intelligence feed, and overall
health of a server monitoring-tool; to create a comprehensive way to secure a system. Project
Artillery was written to be an addition to security on a server and make it very
difficult for attackers to penetrate a system. The concept is simple: project
Artillery will monitor the filesystem looking for indicators of change. If one is
detected, an email is sent to the server owner. Artillery also listens for rogue 
connections. If detected, a notification is sent to the server owner, and
the offending IP address is banned.

Install Location
----------------

`/opt/artillery`

Config File Location
--------------------

`/opt/artillery/config`

Usage
-----

All options are set in the configuration file so there are no command
line arguments for Artillery.

`/var/artillery$` **`sudo python2 artillery.py`**

Video Walkthrough
-----------------

<video controls>
  <source src="Videos/1_550_Artillery.mp4">
  <source src="https://onedrive.live.com/download.aspx?cid=8D6C4317A39E3D29&resid=8D6C4317A39E3D29%2155680&canary=">
 <p>Your browser does not support html5 video.</p>
</video>

Example 1: Running Artillery
----------------------------

Change to the install location and run Artillery. You will need to enter
the password for adhd.

`/var/artillery$` **`sudo python2 artillery.py 2> /dev/null`**

Verify that Artillery is running by opening a new terminal and typing
the following command. You should see that the python2 process is
listening on a bunch of ports. These are the ports that are configured
by default in the config file.

`$` **`sudo netstat -nlp | grep python2`**

        tcp  0  0  0.0.0.0:5900   0.0.0.0:*  LISTEN  9020/python2
        tcp  0  0  0.0.0.0:110    0.0.0.0:*  LISTEN  9020/python2
        tcp  0  0  0.0.0.0:10000  0.0.0.0:*  LISTEN  9020/python2
        tcp  0  0  0.0.0.0:8080   0.0.0.0:*  LISTEN  9020/python2
        tcp  0  0  0.0.0.0:21     0.0.0.0:*  LISTEN  9020/python2
        tcp  0  0  0.0.0.0:1433   0.0.0.0:*  LISTEN  9020/python2
        tcp  0  0  0.0.0.0:1337   0.0.0.0:*  LISTEN  9020/python2
        tcp  0  0  0.0.0.0:25     0.0.0.0:*  LISTEN  9020/python2
        tcp  0  0  0.0.0.0:44443  0.0.0.0:*  LISTEN  9020/python2
        tcp  0  0  0.0.0.0:1723   0.0.0.0:*  LISTEN  9020/python2
        tcp  0  0  0.0.0.0:445    0.0.0.0:*  LISTEN  9020/python2
        tcp  0  0  0.0.0.0:3389   0.0.0.0:*  LISTEN  9020/python2
        tcp  0  0  0.0.0.0:135    0.0.0.0:*  LISTEN  9020/python2
        tcp  0  0  0.0.0.0:5800   0.0.0.0:*  LISTEN  9020/python2

Example 2: Triggering a Honeyport
---------------------------------

Start Artillery as in Example 1. Then find the IP address of the ADHD
machine by using ifconfig.

`$` **`ifconfig`**

        eth0    Link encap:Ethernet  HWaddr 00:0c:29:6c:14:79
                inet addr:192.168.1.137  Bcast:192.168.1.255  Mask:255.255.255.0
                inet6 addr: fe80::20c:29ff:fe6c:1479/64 Scope:Link
                UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
                RX packets:136005 errors:0 dropped:0 overruns:0 frame:0
                TX packets:59528 errors:0 dropped:0 overruns:0 carrier:0
                collisions:0 txqueuelen:1000
                RX bytes:146777599 (146.7 MB)  TX bytes:7955605 (7.9 MB)
                Interrupt:19 Base address:0x2000

        lo      Link encap:Local Loopback
                inet addr:127.0.0.1  Mask:255.0.0.0
                inet6 addr: ::1/128 Scope:Host
                UP LOOPBACK RUNNING  MTU:16436  Metric:1
                RX packets:12930 errors:0 dropped:0 overruns:0 frame:0
                TX packets:12930 errors:0 dropped:0 overruns:0 carrier:0
                collisions:0 txqueuelen:0
                RX bytes:3413486 (3.4 MB)  TX bytes:3413486 (3.4 MB)

In this case the IP address for the machine is 192.168.1.137. Now, using
another machine and either netcat or telnet, connect to the ADHD machine
on port 21. Port 21 is one of the ports Artillery is monitoring.

NOTE: You may want to mute your speakers before running this command if
you are in a place where you could disturb others.

`$` **`telnet 192.168.1.137 21`**

        Trying 192.168.1.137...
        Connected to ubuntu.
        Escape character is '^]'.
        ?Q???y{+g??g?gF?=?>??~??$}k????KU??M
        <<<snip>>>
        ?????P??N?+???T?Z???~0?Connection closed by foreign host.

The reason for muting your speakers should be apparent now.  
Artillery sends a bunch of garbage characters when a connection is made.  
Some of these characters are usually ASCII bell characters that will make 
your computer ding a whole lot.

The result of our connection attempt should be that Artillery
automatically blocked all connections from the remote IP address. Try
connecting again to the ADHD machine using either telnet or netcat.

`$` **`telnet 192.168.1.137 21`**

        Trying 192.168.1.137...
        telnet: connect to address 192.168.1.137: Operation timed out
        telnet: Unable to connect to remote host

As you can see the connection timed out, indicating that we no longer
have access from the remote host we are currently using.

NOTE: If you try to do this using another terminal within ADHD, this
will not work. Artillery whitelists local connections so you can't block
127.0.0.1.

Back on ADHD, open up a new terminal and check syslog for Artillery's logs.

`$` **`tail /var/log/syslog | grep Artillery`**

        <<<snip>>>
        Feb 12 13:38:17 ubuntu 2014-02-12 13:38:17.957044 [!] Artillery has blocked (and blacklisted the IP Address: 192.168.1.193 for connecting to a honeypot restricted port: 445

At the end of the output you should see a log entry similar to the
above. Note the IP address as we will now undo the ban Artillery put
into place. In this instance, the remote IP address is 192.168.1.193.

`/var/artillery$` **`sudo python2 remove_ban.py 192.168.1.193`**

        Listing all iptables looking for a match... if there is a massive amount of blocked IP's this could take a few minutes..
        1

If for some reason the script doesn't work, try running it a few times to unban your IP, 
or simply flush iptables like so:

`/var/artillery$` **`sudo iptables -F`**

It should be noted that the above command will remove all iptables rules.

Example 3: Adding a File to a Watched Directory
-----------------------------------------------

Start Artillery as in [Example 1: Running Artillery]. Open up a new terminal and add a new
file into a watched directory.

`$` **`sudo touch /var/www/bad_file`**

Artillery is set up to check for changes every 60 seconds by default so
the log file may not show the change immediately. Watch for the change
by typing the following command.

`$` **`watch tail -n 15 /var/log/syslog`**

And look for output similar to the following.

        *******************************************************************
        The following changes were detected at 2013-01-15 21:15:38.541391
        *******************************************************************
        1a2
        > /var/www/bad_file: cf83e1357eefb8bdf1542850d66d8007d620e4050b5715dc83f4a921d36ce9ce47d0d13c5d85f2b0ff8318d2877eec2f63b931bd47417a81a538327af927da3e

        ************************** End of changes. ***************************


