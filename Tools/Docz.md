
Docz.py
=======

Website
-------

<https://bitbucket.org/Zaeyx/docz.py>

Description
-----------

Docz is a simple tool used to insert webbugs (Ala WebBugServer) into docx files.
As opposed to the simpler .doc format, a docx formatted document is far harder to analyze.
This means, it's easier for us to hide our bugs from the target.

Install Location
----------------

`/opt/docz.py/`

Usage
-----


Running Docz.py couldn't be easier, simply cd to the correct directory.

`~$` **`cd /opt/docz.py`**

And run the application

`/opt/docz.py$` **`python ./docz.py -h`**

This will show you the very very simple help dialog.

		usage: docz.py [-h] -f FILE -u URL -t TARGET -a AGENT
		
		A script for embedding webbugs in docx files
		
		optional arguments:
		  -h, --help            show this help message and exit
		  -f FILE, --file FILE  The name of the file to embed the bug in
		  -u URL, --url URL     The url to the honeybadger server's service.php
		                        (inclusive)
		  -t TARGET, --target TARGET
		                        The target identifier
		  -a AGENT, --agent AGENT
		                        The agent identifier


Example 1: Creating a Webbug for Honeybadger
--------------------------------------------

For this example we are going to assume you have a honeybadger server set up and listening already.  If you need help doing that, please refer to the documentation for honeybadger before proceeding.

Docz will want us to set a few parameters before we can continue.  The parameters it requests are seen above in the [Usage] Section.  They are as follows...

* -f A docx file
* -u The URL (path) to the honeybadger server
* -t The unique target identifier
* -a The agent identifier

For this example, we will use these example values (your values may be different).

We will assume that we have a docx file created in Microsoft Word with the name "Layoffs2017.docx", stored in the same folder as docz.py.

We will assume that the path to our honeybadger server is as follows (make sure to include the service.php at the end of the path): http://nothoneybadger.blackhillsinfosec.com/honeybadger/service.php

Each target must be assigned a unique target ID.  For more on this see the honeybadger documentation.  We will use the target of "demotarget".

Each target must be assigned a unique agent identifier.  We will use the agent "docx".

We would now call docz on the command line like so.

`/opt/docz.py$` **`python docz.py -f Layoffs2017.docx -u http://nothoneybadger.blackhillsinfosec.com/honeybadger/service.php -t demotarget -a docx`**

		Connection string: http://nothoneybadger.blackhillsinfosec.com/honeybadger/service.php?agent=docx&target=demotarget
		You better have write permissions to this folder, otherwise this is going to fail silently.
		Archive:  Layoffs2017.docx
		  inflating: tmp/[Content_Types].xml
		  inflating: tmp/_rels/.rels
		  inflating: tmp/word/_rels/document.xml.rels
		  inflating: tmp/word/document.xml
		  inflating: tmp/word/theme/theme1.xml
		  inflating: tmp/word/settings.xml
		  inflating: tmp/word/fontTable.xml
		  inflating: tmp/word/webSettings.xml
		  inflating: tmp/docProps/app.xml
		  inflating: tmp/docProps/core.xml
		  inflating: tmp/word/styles.xml
		/root/dev/docz.py/tmp
		  adding: [Content_Types].xml (deflated 74%)
		  adding: _rels/ (stored 0%)
		  adding: _rels/.rels (deflated 61%)
		  adding: docProps/ (stored 0%)
		  adding: docProps/core.xml (deflated 52%)
		  adding: docProps/app.xml (deflated 54%)
		  adding: word/ (stored 0%)
		  adding: word/styles.xml (deflated 90%)
		  adding: word/_rels/ (stored 0%)
		  adding: word/_rels/document.xml.rels (deflated 69%)
		  adding: word/webSettings.xml (deflated 53%)
		  adding: word/fontTable.xml (deflated 69%)
		  adding: word/document.xml (deflated 69%)
		  adding: word/settings.xml (deflated 63%)
		  adding: word/theme/ (stored 0%)
		  adding: word/theme/theme1.xml (deflated 77%)
		
		
		
		*****************************
		Webbug created as output.docx
		*****************************


Docz.py will create a new file called output.docx identical to your input file but with the inclusion of a stealthy webbug that will call back to your honeybadger server whenever the file is opened.  This webbug is especially hard to find because of the way it is embedded inside of the docx file as opposed to a simple doc file.




Example 2: Creating a Webbug for Webbugserver
---------------------------------------------

The process by which docz.py is used to create a webbug for webbugserver or sqlitebugserver is identical to [Example 1: Creating a Webbug for Honeybadger](Example_1:_Creating_a_Webbug_for_Honeybadger).  

Just put in the new URL to your sqlitebugserver, and change any other parameters you may need based on your situation.
