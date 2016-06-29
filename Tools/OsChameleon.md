OsChameleon
=======

Website
-------

<https://github.com/zaeyx/oschameleon>

Description
-----------

OsChameleon is a tool that hides the fingerprint of modern linux kernels from tools such as nmap.

Install Location
----------------

`/opt/oschameleon/`

Usage
-----

Using oschameleon is incredibly easy.  Simply cd into its directory:

`$` **`cd /opt/oschameleon`**

And run the osfuscate.py script as root

`$` **`sudo python ./osfuscate.py`**

It should hang, and you should see no output until someone attempts to scan you.When someone does scan you however, you will see a ton of output as the probes hit your machine and oschameleon responds.


Example 1: Scanning Yourself
---------------------------------

For this example, you will need to have nmap installed.  If you do not have it installed for some reason, you can get it via apt.

`$` **`sudo apt-get install nmap`**

First, without oschameleon running, let's attempt to detect our OS.

Simply run:

`$` **`sudo nmap -O localhost`**

It should take a minute.  And then you'll get back an output that looks something like this.

		Starting Nmap 6.47 ( http://nmap.org ) at 2016-06-12 23:14 EDT
		Nmap scan report for localhost (127.0.0.1)
		Host is up (0.000020s latency).
		Not shown: 996 closed ports
		PORT	 STATE SERVICE
		80/tcp	 open  http
		3306/tcp open  mysql
		Device type: general purpose
		Running: Linux 3.X
		OS CPE: cpe:/o:linux:linux_kernel:3
		OS details: Linux 3.7 - 3.15
		Network Distance: 0 hops

		OS detection performed. Please report any incorrect results at http://nmap.org/submit .
		Nmap done: 1 IP address (1 host up) scanned in 4.18 seconds

This is a pretty simple result we have here.  You can see that nmap was able to fingerprint our system with ease (the lines in question being the ones "OS CPE" and "OS details").

Now let's run oschameleon and try again.

You'll need to open up a new terminal first.  So you have two terminals open.  One for oschameleon and one for nmap.  

In your oschameleon terminal run these commands.

`$` **`cd /opt/oschameleon`**

`/opt/oschameleon$` **`sudo python ./osfuscate.py`**

It shouldn't show you any output yet.

Now in your second terminal run your nmap scan again.

`$` **`sudo nmap -O localhost`**

You should notice oschameleon throwing output to the screen shortly after you begin the scan.  Don't worry about this.  This just shows us that the script is working.  What we're more interested in right now is the result that nmap gives us when it's done.

Your exact result back from nmap map differ.  But it should look quite different now.  

		
		Starting Nmap 6.47 ( http://nmap.org ) at 2016-06-12 23:14 EDT
		Nmap scan report for localhost (127.0.0.1)
		Host is up (0.000020s latency).
		Not shown: 996 closed ports
		PORT     STATE SERVICE
		80/tcp   open  http
		3306/tcp open  mysql
		No exact OS matches for host (If you know what OS is running on it, see http://nmap.org/submit/ ).
		

This is what we're expecting (followed by a lengthy TCP/IP fingerprint section).

That's it.  It's that simple.  Just like that we've disguised our system.


