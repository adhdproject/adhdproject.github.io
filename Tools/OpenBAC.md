
OpenBAC POC
=======

Website
-------

<https://bitbucket.org/Zaeyx/openbac>

Description
-----------

The OpenBAC proof of concept exists to demonstrate the Ball and Chain 
password representation format.  It is very important that you understand
the POC is not cryptographically secure.  Do not use it to store anything
important.

As development on the library continues, this POC will be updated until 
it is the full, final, secure version.

If you're curious as to what Ball and Chain is, please watch this conference
talk describing it in full. 
<https://www.youtube.com/watch?v=GfyM8lFkjo8&feature=youtu.be>


Install Location
----------------

`/opt/OpenBAC/`

Usage
-----

Usage is a multi step process.  The POC is not a traditional one
use script.  It is multiple scripts working in tandem to demonstrate
how an algorithm might function to authenticate users in an ultra secure
manner.

Let's start by initializing the algorithm.

Eample 1: Initialization
------------------------

There are a couple of different parts to running this POC.

The Ball and Chain algorithm creates "password hashes" and stores
them in a "passwd" file.  It also builds a small (example size) array
of pseudorandom data to link the password hashes to.  You need to
initialize the construct in order to build these two elements.

**`cd /opt/OpenBAC/`**

`/opt/OpenBAC$` **`python ./initialize.py`**

Next we will add a user.

Example 2: Adding a user
------------------------

Adding a user couldn't be easier.  
Make sure you are in the correct directory.

**`cd /opt/OpenBAC/`**

Then run "add_user.py" and follow the prompts.

`/opt/OpenBAC` **`python ./add_user.py`**

Put in your username, and your password of choise.  The output will 
reflect everything the script does, including some debug data. 

		[Username]: spectre
		[Password]: pale_king
		
		***Debug Data***
		Data, len: 9c4134c3a6c517cff9ac35b66889413d 32 Address:   19
		Binary Output: 
		
		Username: spectre
		Hash: faa53508e1643045cff236b5b2432fac31

Now you have a user registered.  If you want to check on your user
you can look inside the passwd file generated in the OpenBAC directory.

Example 3: Authentication
-------------------------

There is a final script that allows you to authenticate users.  

`/opt/OpenBAC` **`python ./auth_user.py`**

You can attempt to authenticate with your user using correct and incorrect
passwords.  Everything should work perfectly.  If you manage to find something
weird or out of place, by all means, email ben@prometheaninfosec.com

