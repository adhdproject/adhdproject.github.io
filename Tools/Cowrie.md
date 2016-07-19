
Cowrie
============

Website
-------

<https://github.com/micheloosterhof/cowrie>

Description
-----------

Cowrie is a medium interaction SSH honeypot designed to log brute force attacks and, 
most importantly, the entire shell interaction performed by the attacker.

Cowrie is developed by Michel Oosterhof and is based on Kippo by Upi Tamminen (desaster).

Install Location
----------------

`/opt/cowrie`

Usage
-----

Cowrie is incredibly easy to use.

It basically has two parts you need to be aware of:

  * A config file
  * A launch script

The config file is located at

`/opt/cowrie/cowrie.cfg`


Example 1: Running Cowrie
------------------------

By default Cowrie listens on port 2222 and emulates an ssh server.

To run Cowrie, cd into the Cowrie directory and execute:

`~$` **`cd /opt/cowrie`**
`~$` **`./start.sh`**

        Starting cowrie with extra arguments [] ...

We can confirm Cowrie is listening with lsof:

`~$` **`lsof -i -P | grep twistd`**

        twistd  548 adhd    6u  IPv4 523637      0t0  TCP *:2222 (LISTEN)

Looks like we're good.

Example 2: Cowrie In Action
--------------------------

Assuming Cowrie is already running and listening on port 2222, (if not see [Example 1: Running Cowrie]),
 we can now ssh to Cowrie in order to see what an attacker would see.

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

Example 3: Viewing Cowrie's Logs
-------------------------------

Change into the Cowrie log Directory:

`~$` **`cd /opt/cowrie/log`**

Now tail the contents of cowrie.log:

`/opt/cowrie/log$` **`tail cowrie.log`**

        2016-02-17 21:52:12-0700 [-] unauthorized login: 
        2016-02-17 21:54:51-0700 [SSHService ssh-userauth on HoneyPotTransport,0,127.0.0.1] adhd trying auth password
        2016-02-17 21:54:51-0700 [SSHService ssh-userauth on HoneyPotTransport,0,127.0.0.1] login attempt [adhd/asdf] failed
        2016-02-17 21:54:52-0700 [-] adhd failed auth password
        2016-02-17 21:54:52-0700 [-] unauthorized login: 
        2016-02-17 21:54:53-0700 [SSHService ssh-userauth on HoneyPotTransport,0,127.0.0.1] adhd trying auth password
        2016-02-17 21:54:53-0700 [SSHService ssh-userauth on HoneyPotTransport,0,127.0.0.1] login attempt [adhd/adhd] failed
        2016-02-17 21:54:54-0700 [-] adhd failed auth password
        2016-02-17 21:54:54-0700 [-] unauthorized login: 
        2016-02-17 21:54:54-0700 [HoneyPotTransport,0,127.0.0.1] connection lost

Here we can clearly see my login attempts and the username/password combos I employed as I tried 
to gain access in [Example 2: Cowrie In Action].  This could be very useful!





