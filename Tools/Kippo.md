
Kippo
============

Website
-------

<https://code.google.com/p/kippo/>

Description
-----------

Kippo is a medium interaction SSH honeypot designed to log brute force attacks and, 
most importantly, the entire shell interaction performed by the attacker. Kippo is inspired, 
but not based on Kojoney. (From Website)

Install Location
----------------

`/opt/kippo/`

Usage
-----

Kippo is incredibly easy to use.

It basically has two parts you need to be aware of:

  * A config file
  * A launch script

The config file is located at

`/opt/kippo/kippo.cfg`

Video Walkthrough
-----------------

<video controls>
  <source src="Videos/1_550_Kippo.mp4">
  <source src="https://onedrive.live.com/download.aspx?cid=8D6C4317A39E3D29&resid=8D6C4317A39E3D29%2155666&canary=">
 <p>Your browser does not support html5 video.</p>
</video>

Example 1: Running Kippo
------------------------

By default Kippo listens on port 2222 and emulates an ssh server.

To run Kippo, cd into the kippo directory and execute:

`~$` **`cd /opt/kippo`**
`~$` **`./start.sh`**

        Starting kippo in the background...

We can confirm Kippo is listening with lsof:

`~$` **`lsof -i -P | grep twistd`**

        twistd  548 adhd    7u  IPv4 523637      0t0  TCP *:2222 (LISTEN)

Looks like we're good.

Example 2: Kippo In Action
--------------------------

Assuming Kippo is already running and listening on port 2222, (if not see [Example 1: Running Kippo]),
 we can now ssh to Kippo in order to see what an attacker would see.

`~$` **`ssh -p 2222 localhost`**

        The authenticity of host '[localhost]:2222 ([127.0.0.1]:2222)' can't be established.
        RSA key fingerprint is 05:68:07:f9:47:79:b8:81:bd:8a:12:75:da:65:f2:d4.
        Are you sure you want to continue connecting (yes/no)? yes
        Warning: Permanently added '[localhost]:2222' (RSA) to the list of known hosts.
        Password:
        Password:
        Password:
        adhd@localhost's password: 
        Permission denied, please try again.

It looks like our attempts to authenticate were met with failure.

Example 3: Viewing Kippo's Logs
-------------------------------

Change into the Kippo log Directory:

`~$` **`cd /opt/kippo/log`**

Now tail the contents of kippo.log:

`/opt/kippo/log$` **`tail kippo.log`**

        2014-02-17 21:52:12-0700 [-] unauthorized login: 
        2014-02-17 21:54:51-0700 [SSHService ssh-userauth on HoneyPotTransport,0,127.0.0.1] adhd trying auth password
        2014-02-17 21:54:51-0700 [SSHService ssh-userauth on HoneyPotTransport,0,127.0.0.1] login attempt [adhd/asdf] failed
        2014-02-17 21:54:52-0700 [-] adhd failed auth password
        2014-02-17 21:54:52-0700 [-] unauthorized login: 
        2014-02-17 21:54:53-0700 [SSHService ssh-userauth on HoneyPotTransport,0,127.0.0.1] adhd trying auth password
        2014-02-17 21:54:53-0700 [SSHService ssh-userauth on HoneyPotTransport,0,127.0.0.1] login attempt [adhd/adhd] failed
        2014-02-17 21:54:54-0700 [-] adhd failed auth password
        2014-02-17 21:54:54-0700 [-] unauthorized login: 
        2014-02-17 21:54:54-0700 [HoneyPotTransport,0,127.0.0.1] connection lost

Here we can clearly see my login attempts and the username/password combos I employed as I tried 
to gain access in [Example 2: Kippo In Action].  This could be very useful!


