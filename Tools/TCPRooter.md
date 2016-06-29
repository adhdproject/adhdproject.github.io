TCPRooter
=======

Description
-----------

TCPRooter is a simple script that allows you to mask your available services to an incoming nmap style portscan by making it appear that all scanned ports are open and listening.  

This leaves the attacker clueless as to what is true and what is false.  Wasting his time as he tries to find which ports are actually open.

Install Location
----------------

`/opt/tcprooter`

Usage
-----


Launching tcproter is super simple.  Just cd into the directory.

`~$` **`cd /opt/tcprooter`**

And run the script

`/opt/tcprooter$` **`sudo ./tcprooter`**

This will launch the script

		TCP Ro0ter, Version 1.0
		Author: Joff Thyer, Copyright (c) 2011
		Listening on 10 TCP ports from port 1 through 10.
		0 TCP ports in use by other processes.
		Ready to accept connections!
		
		Press <ENTER> to exit.


Example 1: Increasing Coverage
-----------------------------------------

By default tcprooter listens on only ten ports.  Ports 1 through 10.  We can use a switch however to modify this, increasing coverage.

Just use the `-n` switch when launching tcprooter.

For example, if you wanted to cover the first 10,000 ports you would run...

`/opt/tcprooter$` **`sudo ./tcprooter -n 10000`**

		TCP Ro0ter, Version 1.0
		Author: Joff Thyer, Copyright (c) 2011
		Listening on 9993 TCP ports from port 1 through 10000.
		7 TCP ports in use by other processes.
		Ready to accept connections!
		
		Press <ENTER> to exit.


It's that easy.
