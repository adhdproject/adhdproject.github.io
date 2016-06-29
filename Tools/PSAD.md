PSAD
=======

Website
-------

<http://cipherdyne.org/psad>

Description
-----------

PSAD is an intrusion detection and log analysis tool that runs on the backbone that is iptables.  PSAD itself is a collection of daemons that run to detect port scans and other malicious traffic by analyzing iptables logs.

Install Location
----------------

`/opt/psad/`

Usage
-----


Launching psad is super simple.

You just need to start the service.  (NOTE: You will need to make sure everything is installed first.)

`$` **`/etc/init.d/psad-init start`**

This will start psad, kmsgsd and psadwatchd.

Example 1: Installing PSAD
-----------------------------------------

To get started, we first need to run the psad setup scripts.  This will build you a fresh installation of psad on your system.  Installation is incredibly easy.

So let's get started.  psad comes with its own perl script called "install.pl" that will do everything for you.  However, there are a few things you may need to do.  For starters, you may need to edit some of the configuration lines near the top of the install script.  

However, most likely you will not need to touch the script at all.  The configurations should only need to be messed with if for some reason the install script errors out.  The lines at the top of this script are all about pointing out where on the system certain resources are.  So if the install script errors, it may simply be because it can't find something it needs.  Just patch it up!


Assuming everything will most likely be fine though.  Let's head over to the location of the script and launch it.

Note: depending on your specific setup, you may or may not be required to run the install script as root.  For the sake of this tutorial, we will be assuming you run all commands as root.

`#` **`cd /opt/psad`**

And run the install script.

`/opt/psad#` **`./install.pl`**

It's that easy.  The install script will ask you a few questions.  You may answer them differently depending on the specifics you require.  If in doubt however, simply accept the defaults.

Example 2: Iptables Configuration
----------------------------------------------

Now, iptables as a subject is a monstrosity.  There is simply so much to discuss if we were to attempt to cover all of it.  However for the sake of brevity, today we will simply be talking about the specific requirements psad has for iptables if it is to function correctly.

Your specific iptables setup is your own to determine.  But if you want it to work with psad you will need to set your configuration to log messages for psad to subsequently parse.

For example, you might run these two commands

`#` **`iptables -A INPUT -j LOG`**
`#` **`iptables -A FORWARD -j LOG`**

According to the official psad documentation you would be advised to run these two commands AFTER you have set all ACCEPT rules and BEFORE setting any DENY rules.


Example 3: Checking for Messages
---------------------------------------------

Checking for alerts from psad is quite simple.  To start with you can check to se if you have any new alerts by running:
`#` **`psad --Status`**

		Iptables prefix counters:
		
		   "Inbound": 1

You can also check the contents of /var/log/messages for something similar to

		Jun 15 23:37:33 netfilter kernel: Inbound IN=lo OUT=
		MAC=00:11:d4:38:b7:e5:00:01:5c:22:9b:c2:08:00 SRC=127.0.0.1 DST=127.0.0.1 LEN=60
		TOS=0x10 PREC=0x00 TTL=64 ID=47312 DF PROTO=TCP SPT=40945 DPT=30003 WINDOW = 32767
		RES=0x00 SYN UGRP=0


Example 4: Email Alerts
-------------------------------

There is a ton more to explore in the toolset that is psad.  One feature that may come in incredibly handy to you in the future is email alerting.  

Psad can be configured to send you emails whenever an alert is triggered.

Now, when you first installed psad, during the installation process you were prompted to set addresses to which you wanted alerts sent.  But assuming you did not set the proper addresses, or neglegted to set anything at all, here is how you change this setting later.

Simply edit the file located at **/etc/psad/psad.conf** using your text editor of choice (ex. vim/nano/gedit).  Modifying the line that looks like:

		EMAIL_ADDRESSES		myemail@mydomain.com, mybossesemail@hisdomain.com;

Making sure to seperate emails with a comma, and end the line with a semicolon.  It's that easy.  Then simply restart the psad service.

`#` **`/etc/init.d/psad restart`**


Example 5: Updating Signatures
------------------------------------------

Psad makes use of snort signatures to detect certain types of attacks as they hit your system's firewall.  It is important to keep these signatures up to date.  You are given the option to update them once when you first install psad.  But you'll want to keep them updated atleast occasionally.  

To do this, simply run:

`#` **`psad --sig-update`**

Then restart the psad service.

`#` **`/etc/init.d/psad restart`**



