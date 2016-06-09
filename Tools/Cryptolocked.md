
Cryptolocked
============

Website
-------

<https://bitbucket.org/Zaeyx/cryptolocked>

Description
-----------

Cryptolocked is a file system integrity failsafe.  That is, it monitors your file system for 
unauthorized modification.  And will trigger failsafe countermeasures in the event of an 
unauthorized modification of your filesystem.

Cryptolocked was developed initially as a response to Crypto based ransomware like Cryptolocker.

Cryptolocked uses tripfiles, special files that should never be otherwise modified.  It monitors 
these files for modification or destruction.  The current countermeasures include shutdown, email 
alerts, and a simulated countermeasure (for testing purposes).

Install Location
----------------

`/opt/cryptolocked/`

Usage
-----

To run Cryptolocked navigate to the install location and run the tool as follows.

`~$` **`cd /opt/cryptolocked`**

`/opt/cryptolocked$` **`sudo python2 ./cryptolocked.py`**

This will start Cryptolocked in basic, unarmed mode.

To trigger the simulated failsafe, either modify or delete the tripfile (test.txt) located in the 
directory from which you ran Cryptolocked.
Let's trigger the failsafe.

`/opt/cryptolocked$` **`sudo rm -f test.txt`**

Note in Example 4 that the script requires access to a gmail account. Some accounts will restrict this and Cryptolock will crash. To remove this restriction, log into the listening gmail account, go to [https://www.google.com/settings/security/lesssecureapps](https://www.google.com/settings/security/lesssecureapps), and 'Turn On' access for less secure apps.

Video Walkthrough
-----------------

<video controls>
  <source src="Videos/1_550_Cryptolocked.mp4">
  <source src="https://onedrive.live.com/download.aspx?cid=8D6C4317A39E3D29&resid=8D6C4317A39E3D29%2155677&canary=">
 <p>Your browser does not support html5 video.</p>
</video>

Example 1: Debug Mode
---------------------
From here on out, we will be calling Cryptolocked as root.
To su to root simply and then cd to /opt/cryptolocked:
`~$` **`sudo su -`**

Cryptolocked comes with a debug mode.  It is important to run this debug mode before arming the tool.  
Debug mode is run to make sure that there are no file permission errors or other such things that 
may cause an unnecessary triggering of the failsafe.

To debug Cryptolocked simply add the word "debug" when calling the program from the command line.

`/opt/cryptolocked#` **`python2 ./cryptolocked.py debug`**

        Checking if file exists:        True
        Checking if file created:       True
        Checking if file written:       True
        Checking if file destroyed:     True
        If all "True" functionality is good

Example 2: Arming Cryptolocked
------------------------------

Cryptolocked starts unarmed by default.  This is to make sure that if using destructive or 
dangerous countermeasures, you must explicitely activiate them.

To arm Cryptolocked simply add the word "armed" when calling the program from the command line.

`/opt/cryptolocked#` **`python2 ./cryptolocked.py armed`**

        Checking if tripfile test.txt exists:        False
        tripfile Instantiated

Now, if the tripfile is modified or destroyed, the countermeasures selected inside of the file 
(cryptolocked.py) via configuration will be executed.  By default, this will be to shutdown your 
system to prevent further tampering.

Example 3: Cryptolocked's Tentacles
-----------------------------------

By default Cryptolocked only deploys one tripfile (test.txt).  This can be remedied by activating 
the "tentacles" mode.  This mode increases the number and complexity of the tripfiles; burrowing 
deeper into the operating system and increasing the likelihood of successful monitoring.

To activate tentacles mode simply add the word "tentacles" when calling Cryptolocked 
from the command line.

`/opt/cryptolocked#` **`python2 ./cryptolocked.py tentacles`**

It is important to note, that you can only use one command line argument at a time with Cryptolocked.
As such, if you desire to run tentacles in the armed state, you will need to modify the file 
Cryptolocked.py and change the line `armed_state=False` to `armed_state=True`

Alternatively, you can edit the file cryptolocked.py and change the line `tentacles = False` to 
`tentacles = True` and run with the command line argument "armed".

Example 4: Email Alerts
-----------------------

In addition to shutting the system down in the event of failsafe activation Cryptolocked can email 
you to notify you of the event.

To configure email alerts you will need at least one gmail account.

Open the file /opt/cryptolocked/cryptolocked.py

Modify these lines with the credentials and address to the gmail account:

        fromaddr = "user@gmail.com"
        username = 'username'
        password = 'password'

Next you will add the "**to**" address, this can be the same address as the "**from**" but 
doesn't have to be:

        toaddr = "user@domain.com"

To activate email alerts, simply change this line from `False` to `True`:

        alerts_enabled=False

Finally, you may choose to send, or withhold potentially sensitive operating system information:

        sensitive_alerts=True

(True is the default, set to False if you are dealing with a sensitive system and 
do not want OS details in your email.)


