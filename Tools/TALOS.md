
TALOS
=======

Website
-------

<https://github.com/PrometheanInfoSec/TALOS>

Description
-----------

TALOS is an evolution in the democratization of Active Defense technologies
and methodologies.  It is an Active Defense Framework; allowing for the quick
training and deployment of computer network defenders.  Rather than having
to train for each tool individually, every tool in TALOS can be launched
through the same process.  Modify the options, issue the run command.

TALOS is currently in limited beta release.  And will be recieving many
updates shortly.  Please stay tuned, and don't be afraid to download the 
latest release!

Install Location
----------------

`/opt/TALOS/`

Usage
-----

To run the script, first cd to the TALOS directory.

`~$` **`cd /opt/TALOS`**

And run the application

`/opt/TALOS$` **`python ./talos.py`**

To access the help menu from inside the TALOS shell simply type 'help'.

**`TALOS>>> help`**

		# Available commands
		#  1) help
		#      A) help <module>
		#      B) help <command>
		#  2) list
		#      A) list modules
		#      B) list variables
		#      C) list commands
		#      D) list jobs
		#  3) module
		#      A) module <module>
		#  4) set
		#      A) set <variable> <value>
		#  5) home
		#  6) query
		#      A) query <sqlite query>
		#  99) exit

Example 1: Running a honeyport
------------------------------

Let's take a look at how easy it is to run a honeyport from within TALOS.
We'll go with a basic honeyport.

From the TALOS prompt...
**`TALOS>>> use local/honeyports/basic`**

Next we can view all the items we need to configure before launching 
like so.
**`local/honeyports/basic>>> show options`**

		Variables
		Name		Value		Required	Description
		-------------------------------------------------------------------------
		host					no			Leave blank for 0.0.0.0 'all'
		whitelist	127.0.0.1	no			hosts to whitelist (cannot be blocked)
		port					yes			port to listen on
		-------------------------------------------------------------------------

Note: that the prompt has changed from "TALOS" to "local/honeyports/basic"
this lets us know that we have loaded the honeyports module.


Looks like the only thing we need to set is the default port.

**`local/honeyports/basic>>> set port 4444`**

**`local/honeyports/basic>>> run`**

		Listening...

That's it.

