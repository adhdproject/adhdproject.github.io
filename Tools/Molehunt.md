Molehunt
========

Website
-------

<https://github.com/Prometheaninfosec/Molehunt>

Description
-----------

Molehunt is a tool used to assist in the revealing of insider threats.  You can use it to figure out who is leaking sensitive information from your network before they can do any real damage.

Install location
----------------

`/opt/molehunt/`

Usage
-----

Here's how it works.  Say your company is experiencing leaks of sensitive information, and you want to know who is responsible.  No need to go and shut down operations or do anything drastic.  With molehunt you create a suspect list; a list of people who might be your insider threat.  You then create a document that you're alright with leaking (depending on your situation it might need to be legitimate, or it could be a pure decoy).

Next you need to have some sort of collection service running.  Currently the three supported collection services are honeybadger, sqlitebugserver, and webbugserver.  (These are all included with ADHD.  Check out their documentation to learn more about them.)  You link molehunt with a collection service of your choice.

You'll also link molehunt with a generator.  This is a service that will help molehunt produce the bugged documents (Currently the only supported generator 'docz.py' is included with ADHD.)

You'll also configure monitoring capability for molehunt and tell it all the "whitelisted" IP space.  The way it works, molehunt will create bugged documents that callback over the internet when opened.  It will monitor to see when a callback is made from outside of the whitelisted IP space (aka, all your internal ips are whitelisted, but someone brought the document home and opened it outside of the whitelisted IP space).

You then feed the document and the list into molehunt.  Molehunt will create a unique version of the document for each and every suspect on the list.  The differences are unoticeable to the target.  

All you then need to do is email the documents to the suspects by name.  

Then use molehunt's monitoring capability to check and see if any callbacks have been made from outside of your network (aka, someone leaked that version of the file).  And molehunt will match the unique file back to the name of the suspect on your list.

It's not too hard, I promise.  Just a bit of setup, and you'll be on your way.

To run the script with or without setup simply:

`$` **`cd /opt/molehunt`**

`/opt/molehunt$` **`sudo ./molehunt.py`**

	Error: MON_STRING not set
	Please modify script to set MON_STRING
	Alternatively you can specify on command line
	Use: --mon-string="<monstring>"
	Or, disable mon features with --no-mon
	Exiting..



Example 1: Configuring Molehunt
-------------------------------

There are a few things that must be done before you can use molehunt.  

The good news is that molehunt includes extensive commenting inside of the script to guide you through the process.  

Go ahead and open up the script in your favorite text editor.  (For this tutorial we will use nano, but if you have another your are more comfortable with go with that.)

`/opt/molehunt$` **`sudo nano molehunt.py`**

We can see the extensive commenting right away.  The comments should be enough to guide you through the process with or without this tutorial.  Let's get started.

First we will need to specify a builder string.  This is a string pointing to the generator service and telling molehunt the type of service.

Replace the line **`BUILDER_STRING=None`** with
**`BUILDER_STRING=docz:/opt/docz.py/docz.py`**

Next we have the source string.  This ties your documents back to a collection source.  You have the choice of using a number of sources.  Let's go with webbugserver as it is the simplest.

You will need to know your machine's IP address for this if you want to make the callback from anywhere else on the network.  To find your IP address run the command **`ifconfig`** and note the line `inet addr:<ip here>`.

For example my ADHD machine's IP on my network is 192.168.1.102.

NOTE: If you are virtualizing ADHD make sure that you have the networking set to bridged, or something else that allows for communication between your HOST machine and VM.

NOTE: We are also expecting that your host machine will have a compatible version of microsoft office (or a similar office suite, if it doesn't you may not see the callback).

If you are following this tutorial to do a real deployment please don't forget to change your internal IP to the IP/domain of your machine on the web wherin your collection service lies.

Edit the line **`SOURCE_STRING=None`** to (with your ADHD's IP address)
**`SOURCE_STRING=http://192.168.1.102/web-bug-server/index.php?id=::ID&type=img`**

NOTE: You might have guessed that "::ID" is a special value for molehunt.  That is where molehunt places the unique ID for each callback.  If you ever wanted to modify the callback you would want to make sure to place this value in the right place.

Finally we will set the monitor string.  This is the string molehunt uses to connect to the results of the campaign and map callbacks to names for you.

Change the line **`MON_STRING=None`** to
**`MON_STRING=webbugserver:root:adhd:webbug`**

This specifiese the type of collection service, the mysql username, pass and db.

(Obviously if you've changed these default values, or if you aren't working from a default ADHD install you'll need to modify this.)

There are a few more values you can set.  The only thing we are going to touch in this tutorial is the whitelist. Molehunt ignores all connections that come from witelisted IP space.

In a real world scenario you would fill this list with the IPs of machines that you trusted.  Such as your work computers.  But if someone opened a file on their home computer, that IP wouldn't be blacklisted, so you would get an alert that someone had brought the files homes.

That's it, if everything in your environment matches up with the values you just set, molehunt should be ready to go.

Inside nano you can save your changes and exit by hitting `Ctrl+O` then enter, then `Ctrl+X`.

Example 2: Running a Campaign
-----------------------------

So, now that molehunt is connected to all the services it needs to be, let's get a campaign going.

First we need a target file.  This is a file that contains all the names of suspects you have.. The people you think might be your insider threat.

Let's use cat to write to a simple file.  Run the below command then enter one name per line.  When you're done hit `Ctrl+C`.

`/opt/molehunt$` **`sudo cat > targets.lst`**

		Bond, James
		Bourne, Jason
		Bauer, Jack

Now start molehunt up like so.

`/opt/molehunt$` **`sudo ./molehunt.py`**

		#######################################	
		## MOLEHUNT V1.0 Promethean Info Sec ##
		#######################################


		Welcome, for help type "help".

	>>>


We need to set a few variables inside the molehunt prompt.

Let's start by setting the campaign.
Type "campaign" then "adhd" when prompted.

`>>>` **`campaign`**

	Campaign name: adhd

Next let's set the honeyfile.  This is the document you plan on disseminating to your suspect list.

If you don't have a special file you can use the default file hey.docx.

Type "honeyfile" then "hey.docx" when prompted

`>>>` **`honeyfile`**

	Path to honeyfile: hey.docx

Now we need to tell molehunt where that target file is.

Type "targetfile" then "targets.lst" when prompted.

`>>>` **`targetfile`**

	Path to targetfile: targets.lst


That's it, now you're ready to generate your campaign.

`>>>` **`generate`**

	Generation complete...
	Files saved to: campaign/adhd

Now, let's go look a  our generated campaign.

Exit molehunt.py

`>>>` **`exit`**

cd into the campaign directory.

`/opt/molehunt$` **`cd campaign/adhd`**

Now view the contents.

`/opt/molehunt/campaign/adhd$` **`ls`**

	Bauer,_Jack.docx Bond,_James.docx Bourne,_Jason.docx MAPPING.txt

There it is, a file per target.  Just send each target their corresponding version of the honeyfile (Don't forget to change the name back to something like 'hey.docx' right before you send it... don't want them knowing what you're up to.  But you should probably keep the files named like this right up until you send them to make sure you don't get them mixed up.  That could be bad!).


Example 3: Opening a File
-------------------------

The campaign we generated in [Example 2: Running a Campaign] should allow us to test the functionality of Molehunt using our host machine.

NOTE: As mentioned earlier this will only work if your host machine and ADHD can communicate, if you set the IP correctly, and if your version of office is compatible.

From within the campaign folder run this command to open a simple web server so we can transfer the files to your host machine.

`/opt/molehunt/campaign/adhd$` **`python -m SimpleHTTPServer`**

	Serving HTTP on 0.0.0.0 port 8000 ...

**On your host machine** open a web browser and surf to `http://<ADHD IP>:8000`

Download one of the documents then open it.

If everything is properly configured, you should have just sent a callback to molehunt.

Example 4: Monitoring for Leaks
-------------------------------

And here is how you view that callback.  

Open up molehunt.

`/opt/molehunt$` **`sudo ./molehunt.py`**

You'll need to set all the values from earlier again.  You can do it like we did before in [Example 2: Running a Campaign] or you can set them on the command line like this...

`/opt/molehunt$` **`sudo ./molehunt.py --targetfile=targets.lst --honeyfile=hey.docx --campaign=adhd`**

When molehunt opens with everything set, simply enter

`>>>` **`monitor`**

If everything is set correctly, and if the callback was recieved you should see an alert appear after a few seconds displaying the name of the target that leaked your data!

	Callback Recieved For Bauer, Jack from 192.168.1.142 at 1481611574

The time is listed in epoch time for ease of parsing.  You can translate it to any format time you like.  There are many tools available to do this.
