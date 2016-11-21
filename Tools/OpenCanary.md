OpenCanary
==========

Website
-------

<https://opencanary.org>

Description
-----------

OpenCanary is a daemon that runs serveral canary versions of services that alerts when a service is (ab)used.

Install location
----------------

`/opt/opencanary/`

Usage
-----

To start opencanary cd to the install directory and launch the daemon.

NOTE: If opencanary throws any sort of error while launching, the most likely explanation is that you already have something running on a port it needs.  Either stop that running service or modify the file opencanary.conf to disable the opencanary service on that port.

`~$` **`cd /opt/opencanary`**

`/opt/opencanary$` **`sudo bin/opencanaryd --start`**

This will launch the opencanary daemon. 

Example 1: Deploying OpenCanary as a Service
--------------------------------------------

As was covered in the [Usage] section above.  All you need to do in order t obe able to run opencanary is launch the daemon with the --start flag.
Before we do that however, let's dig a little deeper into what exactly it is that opencanary does.

Head over to the opencanary directory and open up the config file.

`~$` **`cd /opt/opencanary`**

`/opt/opencanary$` **`nano opencanary.conf`**

The file is pretty straightforward.  It's formatted in json, so if you're familar with that this format should make sense.  If not you can probably pick it up pretty quick.

Directives start on the left of the ':' character.  The value on the right.  Commas seperate directives.

Let's look at a service and modify one of it's elements.
Let's look at the FTP service.  Find these lines.

		"ftp.banner": "FTP server ready",
		"ftp.enabled": true,
		"ftp.port": 21,

The options as we can see are quite simple.  We can set the banner (the text that a client sees prior to attempting to authenticate).  We can set the port the service listens on.  And we can set whether or not the service is enabled.

Let's change the banner.

Edit the line to read something more like this: "ADHD FTP Server v0.1".
If you're using nano, then all you need to do to save is hit `Ctrl+O` then `Ctrl+X` to exit.

Now that we've done that, let's spool up the daemon and see if our changes worked.

`/opt/opencanary$` **`sudo bin/opencanaryd --start`**

Now, assuming that opencanary didn't throw any errors, let's connect via ftp.
From the same console type this command:

`/opt/opencanary$` **`sudo ftp localhost`**
		220 ADHD FTP Server v0.1
		Name (localhost:root): 

You can attempt to authenticate by typing in a user name and password.

Then type "quit" to exit the ftp client.

Now let's check your syslog to see if opencanary reported on the attempted intrusion.

`/opt/opencanaryd$` **`sudo tail -n 15 /var/log/syslog`**

		SYSLOG HERE
		.
		.
		.
		.
		.
		.
		.
		.
		.
		.
		.
		.
		.

That's it.

To kill opencanary simply:

`/opt/opencanary$` **`sudo killall twistd`**
