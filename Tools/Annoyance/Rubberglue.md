
Rubberglue
=======

Website
-------

<https://github.com/adhdproject/rubberglue>

Description
-----------

Rubberglue is an evolution of the honeyports concept.

However, it takes honeyports one step further.  The name came from the
schoolyard saying "I'm rubber and you're glue.  Everything you say bounces
off of me and sticks to you."  And that's exactly what rubberglue does.

You can set Rubberglue to listen on a port.  Any traffic it recieves on that
port it will forward back to the client on the same port.

So if you set rubberglue to listen (for example) on port 21.  When an
attacker comes in, and connects to your box on port 21, it will redirect
back to the attacker's box.  Better yet, if the attacker actually has
an ftp server on port 21, rubberglue will record all the traffic
passed through it's pipe.  So you can watch and giggle as the attacker
hacks themself.

MITRE Shield
------------

Applicable MITRE Shield techniques:
* [DTE0016](https://shield.mitre.org/techniques/DTE0016) - Decoy Process
* [DTE0026](https://shield.mitre.org/techniques/DTE0026) - Network Manipulation
* [DTE0023](https://shield.mitre.org/techniques/DTE0023) - Migrate Attack Vector
  * Not so much migrating, as it is mirroring
* [DTE0027](https://shield.mitre.org/techniques/DTE0027) - Network Monitoring

Install Location
----------------

`/opt/rubberglue/`

Usage
-----


Launching rubberglue is super simple.  Just cd into the directory.

`~$` **`cd /opt/rubberglue`**

And run the script

`/opt/rubberglue$` **`python3 ./rubberglue.py`**

The help dialog will be displayed.

		You need to give a port
		Usage: rubberglue.py <port>

Example 1: Setting a Trap
-----------------------------------------

For this example, let's create a simple trap with Rubberglue.

The usage is very simple.  If we want to have rubberglue listen on port 4444
we just run it like this.

`/opt/rubberglue` **`sudo python3 ./rubberglue.py 4444`**

That's it.

NOTE: I'm assuming you'll want to test this.  If you choose to, I would recommend not testing the script from the same machine you have it running on.  This can create an infinite loop.  Just go grab a different machine on the network and use netcat or telnet or ssh or whatever to attempt to connect.  Remember rubberglue is going to forward the connection back to your machine on the same port.  So if you have ssh listening on your "attacker" machine, and you have rubberglue listening on port 22 on your "victim" machine, when you connect the connection will be funneled back to your attacker's machine on port 22 and when you try to login, you'll be logging into yourself.
