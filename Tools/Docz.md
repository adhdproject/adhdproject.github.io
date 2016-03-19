
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

`/opt/docz.py$` **`python ./docz.py`**

This will show you the very very simple help dialog.

		Usage: docz.py <filename> <remote_address>

All you need to do is supply docz.py with the path to your docx file as your first argument;
and a remote address in the format "http://my_hostname_or_ip/path_to_listener.php"
As you can guess, you will need to figure out what the remote address/path should look like.
Assuming you have a webbug server running and ready to go, just point to its listener.

Running this will patch up the docx file with the hidden bug.  The target won't be able to easily
analyze the file and determine if there is or is not a bug.

It's that simple.

