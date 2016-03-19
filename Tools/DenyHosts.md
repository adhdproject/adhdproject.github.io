
DenyHosts
=========

Website
-------

<http://denyhosts.sourceforge.net>

Description
-----------

DenyHosts is a Python script written by Phil Schwartz that analyzes your service logs to uncover 
attempts to hack into your system.  Upon discovering a repeatedly malicious host, the `/etc/hosts.deny` 
file is updated to prevent future break-in attempts from that host. 


Install Location
----------------
`/opt/denyhosts`

`/usr/share/denyhosts/`

Example 1: Installing DenyHosts
-------------------------------

`~$` **`sudo apt-get install denyhosts`**

It really doesn't get much simpler than that.

Example 2: Enabling DenyHosts
-----------------------------

To enable DenyHosts, simply start its service.

`~$` **`sudo /etc/init.d/denyhosts start`**

Example 3: Basic Configuration
------------------------------

A majority of DenyHosts’ configurations can be made by editing the configuration file 
`/etc/denyhosts.conf`.

DenyHosts makes use of the default Linux whitelist and blacklist.

With a blacklisting service like DenyHosts it can be incredibly important to properly configure 
your whitelist prior to launch.  

The default whitelist file for Linux is `/etc/hosts.allow` (this can be changed in the DenyHosts conf file).  

The rule structure is the same for the files `/etc/hosts.deny` (blacklist) and `/etc/hosts.allow` (whitelist).

The pattern is `<service> : <host>`

You will have to be root to run any of the following commands by default.

So for example, if you wanted to allow access to a vsftp service for connections from ‘192.168.1.1’:

`~$` **`sudo su`**

`~#` **`echo “vsftpd : 192.168.1.1” >> /etc/hosts.allow`**

To whitelist a specific host’s connection to all services (example: 192.168.1.1):

`~#` **`echo “ALL : 192.168.1.1” >> /etc/hosts.allow`**

The `ALL` selector can also be used to whitelist or blacklist all hosts on a specific service:

`~#` **`echo “sshd : ALL” >> /etc/hosts.allow`**

