
Bear Trap
=========

Website
-------

<https://github.com/chrisbdaemon/BearTrap>

Description
-----------

BearTrap is meant to be a portable network defense utility written entirely 
in Ruby. It opens "trigger" ports on the host that an attacker would connect 
to. When the attacker connects and/or performs some interactions with the 
trigger an alert is raised and the attacker's IP address is potentially blacklisted.

Install Location
----------------

`/opt/beartrap/`

Config Location
---------------

`/opt/beartrap/config.yml`

Usage
-----

`/opt/beartrap$` **`sudo ruby bear_trap.rb`**

        BearTrap v0.2-beta
        Usage: bear_trap.rb [-vd] -c <config file>
        OPTIONS:
        --config  -c <config file>    Filename to load configuration from [REQUIRED]
        --verbose -v                  Verbose output
        --debug   -d                  Debug output (very noisy)

Example 1: Basic Usage
----------------------

Change into the Bear Trap install directory and start Bear Trap.

`/opt/beartrap$` **`sudo ruby bear_trap.rb -c config.yml`**

Now find the ADHD machine's IP address by opening a new terminal and
using ifconfig.

`$` **`ifconfig`**

        eth0    Link encap:Ethernet  HWaddr 00:0c:29:6c:14:79  
                inet addr:192.168.1.137  Bcast:192.168.1.255  Mask:255.255.255.0
                inet6 addr: fe80::20c:29ff:fe6c:1479/64 Scope:Link
                UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
                RX packets:115178 errors:0 dropped:0 overruns:0 frame:0
                TX packets:43571 errors:0 dropped:0 overruns:0 carrier:0
                collisions:0 txqueuelen:1000 
                RX bytes:104926397 (104.9 MB)  TX bytes:4321023 (4.3 MB)
                Interrupt:19 Base address:0x2000 
    
Here the IP is 192.168.1.137. From a remote computer use FTP to connect
to the ADHD machine. If it asks for a username, it doesn't matter what
you type as Bear Trap doesn't actually implement a real FTP server.
'adhd' is used in the example below.

`$` **`ftp 192.168.1.137`**

        Connected to 192.168.1.137.
        220 BearTrap-ftpd Service ready
        Name (192.168.1.137): adhd
        421 Service not available, remote server has closed connection.
        ftp: Login failed

As you can see, the login failed. Type `quit` to exit the FTP client.

        ftp\> quit

Back in your Bear Trap terminal, you should see new output similar to
below. Bear Trap automatically blocked your remote IP address (in this
case 192.168.1.194) using iptables.

        Command: /sbin/iptables -A INPUT -s 192.168.1.194 -j DROP

If you try to connect again using FTP from your remote computer, the
operation will time out.

`$` **`ftp 192.168.1.137`**

        ftp: Can't connect to '192.168.1.137': Operation timed out
        ftp: Can't connect to '192.168.1.137'

To unblock the remote computer, open a new terminal on the ADHD machine
and type the following iptables command, substituting in your remote IP
address.

`$` **`sudo iptables -D INPUT -s 192.168.1.194 -j DROP`**


