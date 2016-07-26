
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

TALOS is currently in limited alpha release.  And will be recieving many
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

		####################################################
		####################################################
		########  _____ ___   _     _____ _____    #########
		######## |_   _/ _ \ | |   |  _  /  ___|   #########
		########   | |/ /_\ \| |   | | | \ `--.    #########
		########   | ||  _  || |   | | | |`--. \   #########
		########   | || | | || |___\ \_/ /\__/ /   #########
		########   \_/\_| |_/\_____/\___/\____/    #########
		########                                   #########
		####################################################
		########  Promethean Information Security  #########
		####################################################
		##         Welcome to TALOS Active Defense        ##
		##             Type 'help' to begin               ##
		####################################################


To access the help menu from inside the TALOS shell simply type 'help'.

**`TALOS>>> help`**

		# Available commands
		#  1) help
		#     A) help <module>
		#     B) help <command>
		#  2) list
		#     A) list modules
		#     B) list variables
		#     C) list commands
		#     D) list jobs
		#     E) list inst_vars
		#  3) module
		#     A) module <module>
		#  4) set
		#     A) set <variable> <value>
		#  5) home
		#  6) query
		#     A) query <sqlite query>
		#  7) read
		#     A) read notifications
		#     B) read old
		#  8) purge
		#     A) purge log
		#  9) invoke
		#     A) invoke <filename>
		#  10) update
		#  99) exit


Example 1: Running a Honeyport
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
		tripcode			no			tripcode trigger for automation
		-------------------------------------------------------------------------

Note: that the prompt has changed from "TALOS" to "local/honeyports/basic"
this lets us know that we have loaded the honeyports module.


Looks like the only thing we need to set is the default port.

**`local/honeyports/basic>>> set port 4444`**

**`local/honeyports/basic>>> run`**

		Listening...

That's it.


Example 2: Backgrounding Modules & Reading Notifications
--------------------------------------------------------

Some modules in TALOS are written to be able to send notifications back to the command console.  This might can be incredibly useful in detecting and thwarting an attack on your network.  

One of the modules capable of sending notifications back to the command console is the module used in the previous example [Example 1: Running a Honeypot] "local/honeyports/basic".

In this example we will initiate a connection to our honeyport and observer the incoming notification.

Please run the module as you did in the previous example [Example 1: Running a Honeypot] barring one minor difference!  When the module is ready to run (that is, you have set the options and are ready to type "run") instead of typing run, type run -j.  This will launch the module in the background, leaving your prompt inside the main TALOS console rather than migrating it to the module.  The module will execute in the background as before.  

Once you have the module running, open another terminal and connect to the honeyport using netcat, like so.

`$` **`nc localhost 4444`**

The attempted connection may hang (appear to freeze and do nothing).  You can terminate your attempt to connect by pressing Ctrl+C if it does this.


Back inside your TALOS console, your notification should have arrived.  If it has not, just wait a minute and it will.  Looking back at the TALOS prompt you should now see something along the lines of...

		You have received 1 new notification
		1 total unread notifications
		command is: read notifications

Let's read the notification.

`local/honeyports/basic>>>` **`read notifications`**

		2016-07-14 15:55:53.207390:honeyports/basic connection from 127.0.0.1:52079
		2016-07-14 16:04:22.465195:honeyports/basic connection from 127.0.0.1:52081


The command `read notifications` will show you all currently unread notifications.  If you need to see a notification you have read previously you can issue the command `read old`.

`local/honeyports/basic>>>` **`read old`**

		#2016-07-14 15:55:53.207390:honeyports/basic connection from 127.0.0.1:52079
		#2016-07-14 16:04:22.465195:honeyports/basic connection from 127.0.0.1:52081

You can also view the log file.  It is located (from the talos directory) in logs/notify.log



Example 3: Aliases & Autocomplete
------------------------------------

**Aliases**

TALOS comes with many useful features to assist you.  One of those features that is included to make your life easier and your network operations faster is the combination of aliases and autocomplete.

It is quite hard to constantly be learning a whole new collection of commands for each and every framework/tool that you need to use.  As such, TALOS has a robust alias system baked into the interpreter.  

You can learn the TALOS commands.  Or you can use the alias that allow you to speak to the interpreter in different ways.  

For example, the TALOS command to load a new module is `module`.  But if you want to, there are aliases you can use instead of this command.  For example you could load a module with the commands `load`, `use`, or even (to emphasise the file system like nature of the modules) `cd`.  

The command to show what variables can be modified for a module is `list variables`.  But you can use some other aliases such as `show options`, `show variables`, `list options` or even `ls`.  

You can add your own aliases too.  That way if you have a framework you're more comfortable with, and want to speak to TALOS in the same way you speak to it, you can.  Or perhaps you want to build your own list of single character shortcuts to make your hacking even faster.  You can do that.

Simple edit the `aliases` file located in the `conf` directory.  You can append your new alias like so: 
		
**`myalias, command`**

For example:

**`open, module`**

It's that easy.

**Autocomplete**

Now, let's briefly talk about the autocomplete, and the way it works with the aliases feature.  At the time of the creation of this document, the autocomplete has three tiers of commands.  It will likely be far more fine grained in the future.  Those tiers are "loaders", "commands", and "seconds".  Don't worry too much about this.  What's important for you to know is that any aliases you add will automatically be added to the autocomplete system in the same tier as the command they alias.  

To try out the autocomplete system, simply go into the TALOS prompt and hit **TAB**.  The autocomplete is intelligent, based on the tiers mentioned above it can guess what you're trying to write next, and supply you with a list of commands to choose from.  

For example, if you go to the prompt and type `load ` then hit **TAB** twice the autocomplete will spit out a list of modules avaiable to **load** since it assumes that's what you intend to type next.  

`TALOS>>>` **`load `**

		deploy/phantom/ssh/basic             local/honeyports/basic_multiple
		deploy/phantom/ssh/basic+            local/honeyports/invisiports
		deploy/phantom/ssh/multi             local/honeyports/rubberglue
		generate/phantom/basic               local/listener/phantom/basic
		generate/wordbug/doc                 local/listener/phantom/basic_bak
		generate/wordbug/docz                local/listener/phantom/multi_auto
		local/detection/human_py             local/listener/webbug/local_save
		local/detection/simple-pivot-detect  local/listener/webbug/no_save
		local/honeyports/basic               local/spidertrap/basic

That is a basic rundown of the autocomplete and alias system within TALOS.

Example 4: Basic Scripting
--------------------------

TALOS is at its most basic level, simply an interpreter.  It takes in commands from you the user via the prompt, and converts those commands into some sort of output based on the rules specified within the framework.  Ex.  If you ask TALOS for help, you will get this response:

`TALOS>>>` **`help`**

		# Available commands
		#  1) help
		#     A) help <module>
		#     B) help <command>
		#  2) list
		#     A) list modules
		#     B) list variables
		#     C) list commands
		#     D) list jobs
		#     E) list inst_vars
		#  3) module
		#     A) module <module>
		#  4) set
		#     A) set <variable> <value>
		#  5) home
		#  6) query
		#     A) query <sqlite query>
		#  7) read
		#     A) read notifications
		#     B) read old
		#  8) purge
		#     A) purge log
		#  9) invoke
		#     A) invoke <filename>
		#  10) update
		#  99) exit


One thing that you can do with TALOS to make your life even easier, is to script up certain functions.  

For example, if you find yourself constantly needing to launch a honeyport (simple example) you can write all the commands out to a script, and then simply call that script to perform the task.  

To launch a honeyport on port 445 we would write out a script that looks like this:

		load local/honeyports/basic
		set port 445
		run -j

We then have two choices for launching this script.

First, we can launch this script when we launch TALOS by specifying the `--script` option.  Like so:

`/opt/talos#` **`./talos.py --script=/path/to/my/script`**

Or, if we're already inside the TALOS interpreter, we can launch the script using the invoke command.

`TALOS>>>` **`invoke /path/to/my/script`**



Example 5: Tripcodes
--------------------

TALOS comes with a useful automation feature that allows you to launch scripts in response to the triggering of modules on your network.  

This functions using something called "tripcodes".  Certain modules can accept a tripcode as a variable before they're launched.  An example of a module with such a capability is local/honeyports/basic.

If let's take a look at this module.  From within the TALOS prompt issue these commands.

`TALOS>>>` **`use local/honeyports/basic`**

`local/honeyports/basic>>>` **`list variables`**

		Name      Value             Required  Description
		----------------------------------------------------------------------------
		host                        no        Leave blank for 0.0.0.0 'all'
		whitelist 127.0.0.1,8.8.8.8 no        hosts to whitelist (cannot be blocked)
		port                        yes       port to listen on
		tripcode                    no        tripcode trigger for automation
		----------------------------------------------------------------------------


We can see that there is an option here for a "tripcode".  

Here is how that works.  The module will take anything you write into that box, and store it while it runs.  In if someone triggers the honeyport (by visiting it) the module will "phone home" back to TALOS with the tripcode specified.  TALOS will then check to see if there is a script mapped to the tripcode it just received.  If there is, TALOS will launch it.  

So let's add a tripcode.

`local/honeyports/basic>>>` **`set tripcode testinginprogress`**

Before we can launch we also need to set a port.

`local/honeyports/basic>>>` **`set port 1337`**

Everything should be good now.  Let's launch our module in the background.

`local/honeyports/basic>>>` **`run -j`**

In another terminal now, let's explore what we did when we set that specific tripcode.
Navigate to the TALOS directory and open up the file `mapping`.

`#` **`cd /opt/talos`**

`/opt/talos#` **`cat mapping`**

		###The format for this file is simple
		# It goes: <tripcode>,<script>
		# So for example, with tripcode: aaaa
		# and script talos/fightback
		# You would write: aaaa,talos/fightback
		# NOTE: script paths should be relative from the scripts folder
		###

		testinginprogress,talos/honeyport_basic_445

It looks like the tripcode "testinginprogress" is the default tripcode specified in the mapping file.  The mapping file is the place TALOS looks to map tripcodes to scripts.  

In this case, the tripcode testinginprogress is mapped to a template script located in the directory `scripts/talos`.

We can guess what this script does based on its name.  

You could edit this file to add your own tripcodes, and map them to scripts that you create.  

Let's trigger our tripcode.

Firstly we want to see that there's nothing listening on port 445 (You will need to be root for this, or use sudo).

`/opt/talos#` **`lsof -i -P | grep 445`**

You shouldn't see anything.

Now, attempt to connect to your honeyport

`/opt/talos#` **`nc localhost 1337`**

If the connection hangs simply hit ctrl+C.

If we run lsof again...

`/opt/talos#` **`lsof -i -P | grep 445`**

		python  17565     root    3u  IPv4 19923014      0t0  TCP *:445 (LISTEN)


Example 6: Advanced Scripting
-----------------------------

TALOS as a project is still in its infancy.  As such, there isn't a ton of documentation to cover the new features constantly being added to the framework.  In this section we will briefly touch on some of the features that were just added at the time of this document's publication.

**Variables**

Obviously, TALOS has built in support for variables.  Earlier in this walkthrough, we learned how to set variables for a specific module.  But did you know you can also set global variables?  

You can set global variables by setting them without a module loaded.  (From the TALOS prompt.)

Either start TALOS fresh, or if you are inside of TALOS and have a module loaded you can use the `unload` command to go back to the TALOS prompt.

Once your prompt looks like this:
`TALOS>>>` you are good to go.  (This means you do not currently have a module loaded.)

Issue the `set` command, and any variables you set will be added to the global context.

`TALOS>>>` **`set test testy`**

`TALOS>>>` **`list variables`**

		Name  Value  Required  Description
		----------------------------
		test  testy  no        Empty
		----------------------------

There are three variable contexts inside of TALOS.  Global, local, and remote.  

Each is accessible by a different variable preface.  Global variables are prefaced with a `%` (percent sign).  Local variables are prefaced with a `$` (dollar sign).  Remote variables are prefaced with an `@` at symbol.

Let's load up a module, and watch this context difference in action.

`TALOS>>>` **`use local/honeyports/basic`**

		loading new module in load_module

We can use the echo command to see the contents of our variables.

First let's echo a local (module specific) variable.

`local/honeyports/basic>>>` **`echo $whitelist`**

		127.0.0.1,8.8.8.8

Now let's echo that global variable we set earlier.

`local/honeyports/basic>>>` **`echo %test`**

		testy

Make sense?  Whatever variables are needed by the currently loaded module can be accessed in the local context.  The other variables are stored in the background.

But wait! What about the remote context?  I'm glad you asked.  The remote context is an append only context used by query modules launched from phantom.  We haven't covered phantom just yet.  But let's talk about it briefly.

Phantom as a tool is a part of TALOS.  It is an agent that can be deployed on remote systems.  You can then push scripts to Phantom to run them on the remote system.

For example, if you needed to deploy a honeyport on a remote system on the other side of your network, you could use phantom to do that without having to install TALOS there.

There are special modules in phantom called "query modules" that run a task, and return the result.  We're not going to cover their use here.  But what happens with them is they write into the remote context.  They can not read there, nor can they overwrite, they can only append.

You can then access what data is being returned by Phantom using the variable preface @.  

**Comments**

TALOS can accept comments in scripts.  Just prepend your line with a `#` (pound sign) and TALOS will ignore it.

**Conditionals**

TALOS can accept conditional statements in scripts in the form of **if**s

You can write these into your scripts like so:

		if 1 == 1
		echo 1
		echo 2
		echo 3
		fi

Don't be afraid to use variables inside these conditionals.

		if $count == 1
		exit
		

**Goto Statements**

TALOS accepts goto statements inside of scripts.  Place a marker (usually a line you have commented out).  Then jump to it.

		#gohere
		echo 1
		goto #gohere

**Loops**

You can increment and decrement variables in TALOS.  Combine this with ifs and gotos and you can create loops.

		set count 10
		#gohere
		if $count > 0
		dec count
		echo $count
		goto #gohere
		fi
		

**Helper Commands**

There are a selection of commands baked into the interpreter for the express purpose of assisting scripting.  A list of some of the newer commands is included here:

		del <var> --> Delete a variable
		copy <from> <to> --> Copy a var to another var
		cat <var0> <var1> --> concat two vars together
		dec <var> --> decrement the value of a var
		inc <var> --> increment the value of a var
		shell <command> --> execute a shell command
		wait <seconds> --> pause the script
		echo <value> --> echo a value
		echo <var> --> echo a var
		echo::vars_store --> echo global variable context
		echo::variables --> echo local variable context
		invoke <path/to/script> --> invoke a script (think functions)
		put <variable> <value> --> put a value into a variable (append)
		pop <variable> --> pop last value from variable
		isset <variable> -->  check if a variable is set
		length <variable> --> get length of variable
		set <variable> <value> --> set a variable
		query <your_query> --> query the database



Example 7: Phantom Basics
-------------------------

Finally, let's cover the basic of Phantom.  What it is, and how to use it.  

Phantom is a "long arm" module for TALOS.  It is an agent, made to be deployed on your infrastructure, that calls back TALOS and accepts commands from TALOS.  You can push all sorts of commands to Phantom.  

In short, Phantom is to TALOS as Meterpreter is to Metasploit.  That while Metasploit is an offensive tool.  TALOS is a tool designed to assist computer network defenders in the protection of their own assets.

But you can't always expect a network defender to have TALOS installed on every single machine across his entire network.  That's where Phantom comes in.  You can install TALOS on your workstation.  Then use phantom to deploy modules to anywhere you have access.  

For this example, we're going to use one of Phantom's deploy modules.

From within the TALOS prompt, issue this command to load it:

`TALOS>>>` **`use deploy/phantom/ssh/multi`**

		loading new module in load_module

This module exists to seamlessly deploy one or more Phantom instances across your network via SSH.  
Next let's take a look at the options.

`deploy/phantom/ssh/multi>>>` **`show options`**

		Variables
		Name     Value  Required  Description
		------------------------------------------------------------------
		username        yes       Username to login with
		commands        no        Commands to send to deploys
		rhosts          yes       too long, to view type 'more <variable>'
		lhost           yes       The host to call back to
		custom          no        Custom script to use, blank for default
		lport    1226   yes       The port to call back to
		ex_dir   /tmp   yes       directory to execute from (think privileges)
		password        yes       password to login with
		rport    22     yes       Port to connect to
		listen   no     yes       Want to start listening?
		------------------------------------------------------------------

As you can see, there are a number of variables we will need to set here before we can deploy.  Lets go over what each of the ones we need to set accomplish.

		* username is the username to authenticate with on the remote host
		* password is the password to authenticate with
		* commands are optional commands to have phantom execute immediately
		* rhosts is a list of hosts to deploy to
		* lhost is the listening host Phantom should call back to (your box)
		* lport is the listening port Phantom should call back to
		* rport is the remote ssh port to connect to (default: 22)
		* listen tells TALOS whether or not to start a listener automatically

Let's start setting values.  I am going to deploy Phantom in this case to my local system.  Your local system may or may not have SSH installed.  Installing it is outside of the scope of this tutorial.  

You will likely need to set different values than I am setting.  But if you understand what each value does, that shouldn't be a problem.

`deploy/phantom/ssh/multi>>>` **`set username adhd`**
`deploy/phantom/ssh/multi>>>` **`set password adhd`**
`deploy/phantom/ssh/multi>>>` **`set rhosts 127.0.0.1`**
`deploy/phantom/ssh/multi>>>` **`set lhost 127.0.0.1`**
`deploy/phantom/ssh/multi>>>` **`set listen yes`**

You should now be good to launch.  Simply issue the run command, sit back, and relax.

		No custom script specified, building default..
		Attempting to push to: 127.0.0.1
		Command List: ['']
		Press Ctrl + C to exit...
		# Attempting to upload script..
		Script uploaded!
		Attempting to execute script..
		New session established


Now we can interact with the session we have established.

`#` **`interact 1`**

Let's push a module to it.  For this example we will use a basic honeyport.

Currently, this is a multi step process.

1) We load the module locally.
2) We push a copy of the module to phantom
3) We edit the variables locally
4) We push the variables to phantom which launches the module

First we will load the module locally
`S1>>` **`module local/honeyports/basic`**

Now we will push a copy to the Phantom instance
`S1>>` **`push`**

Now let's list the variables to see what we need to set
`S1>>` **`list variables`**

		---------------------------------------------------------------------------
		host                        no        Leave blank for 0.0.0.0 'all'
		whitelist 127.0.0.1,8.8.8.8 no        hosts to whitelist (cannot be blocked
		tripcode                    no        Tripcode to trigger script
		port                        yes       port to listen on
		-------------------------------------------------------------------------

We will set the port we want
`S1>>` **`set port 31337`**

And finally, launch the module
`S1>>` **`launch`**

In another terminal on your system you can confirm that the module was successfully launched by checking to see if something is listening on the port you specified.

`/opt/talos#` **`lsof -i -P | grep 31337`**

		python 21344 	adhd	10u   IPv4   1994461		0t0   TCP  *:31337	(LISTEN)

NOTE: if you run into any errors with the uploading or execution of your script, it is likely related to file permissions.  Try tweaking the permissions of your user, or changing the ex_dir value prior to launch.  (For example, changing ex_dir from `/tmp` to `/home/adhd`.)


There is a ton more to discover inside of TALOS.  So get busy and get exploring.  New content is constantly being added to this project.
