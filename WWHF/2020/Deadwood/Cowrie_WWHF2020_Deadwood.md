Cowrie
======

Introduction
------------
Cowrie is a fun SSH honeypot tool. We've slightly modified our version of Cowrie
to bring back a feature that we were missing: Preventing the attacker from
exiting the honeypot once they log in.

Cowrie runs on port 2222 by default, and allows for "root" login without a
password, as well as several easily guessable or brute-forcible passwords.
Passwords can also be configured. Cowrie simulates some commands to further fool
an attacker into getting comfortable and trying to run system commands.

These three labs provide a brief introduction to Cowrie. To start Cowrie, run
`/opt/cowrie/bin/cowrie start`.

Lab 1
-----
First, log in to Cowrie as root via localhost on port 2222. You should be
presented with a flag. This is a good opportunity to see what Cowrie offers in
its simulated file system.

Cowrie will automatically close your connection after a bit of inactivity.
Either wait for your session to time out naturally, close the terminal window,
or strike `enter` and send SSH magic (`~.`) to force the session to close.

Lab 2
-----
Cowrie captured a login to the honeypot. Looks like the attacker logged in, did
a few things, and then `echo`'d something interesting.

Check the Cowrie log files `cowrie.log.wwhf2020_lab2` or
`cowrie.json.wwhf2020_lab2` and see what the attacker attempted to do before
their SSH session died. There's a flag hiding within the noise.

Lab 3
-----
Cowrie captured another login to the honeypot. This time, the attacker seemed
suspicious, and attempted to exit the honeypot after some commands didn't seem
to function quite right. However, the attacker failed to notice that their
prompt wasn't right after exiting.

Check the Cowrie log files `cowrie.log.wwhf2020_lab3` or
`cowrie.json.wwhf2020_lab3`. The attacker wasn't so careful in checking where
they were logged into and they `echo`'d something strange to the console. Could
there be a flag hidden there somehow?
