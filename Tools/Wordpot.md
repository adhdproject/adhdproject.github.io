
Wordpot
=======

Website
-------

<https://github.com/gbrindisi/wordpot>

Description
-----------

Wordpot is a script you can use to detect bots and others scanning for 
wordpress installations.  Wordpress has historically been somewhat vulnerable
as such, many virtual thieves and criminals actively seek out wordpress.

Wordpot simulates a full install.  Including fake installed themes and plugins.

Install Location
----------------

`/opt/wordpot/`

Usage
-----

To run the script, first cd to the wordpot directory.

`~$` **`cd /opt/wordpot`**

And run the application

`/opt/wordpot$` **`sudo python ./wordpot.py --help`**

This will show you the help dialog.

		Usage: wordpot.py [options]
		
		Options:
		-h, --help			show this help message and exit
		--host=HOST			Host address
		--port=PORT			Port number
		--title=BLOGTITLE	Blog title
		--theme=THEME		Default theme name
		--plugins=PLUGINS	Fake installed plugins
		--themes=THEMES		Fake installed themes
		--ver=VERSION		Wordpress version
		--server=SERVER		Custom "Server" header


Example 1: Running a fake Wordpress install
-------------------------------------------

Wordpot runs by default without any additional configuration required.
Simply run the script with whatever command line arguments you need for
your situation.  With ADHD, by default you should have apache listening
on port 80.  So you might need to specify an alternative port number, or
to disable apache for the time that you're running this script.

`/opt/wordpot$` **`sudo python ./wordpot.py --port=4444`**

It's really that easy.

You can view your fancy new fake wordpress install by opening your web
browser and navigating to "http://localhost:4444"

![Wordpress](Wordpot_files/Wordpot_0.png)
