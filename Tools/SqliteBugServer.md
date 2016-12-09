
Sqlite Bug Server
==============

Website
-------

<https://bitbucket.org/zaeyx/sqlitebugserver>

Description
-----------

Easily embed a web bug inside word processing documents. These bugs are
hidden to the casual observer by using things like linked style sheets and 1 pixel images.

Sqlitebugserver is nearly identical to webbugserver.  But uses an sqlite database
rather than a served mysql database.  This makes it exceptionally easy to rapidly
deploy sqlitebugserver anywhere, at any time.


Install Location
----------------

`/opt/sqlitebugserver/`

Usage
-----
NOTE: You will need to deploy the content from /opt/sqlitebugserver onto your webserver if you want to use sqlitebugserver.  Then visit your deployed index.php.  

This page is intended for receiving Word web bugs as detailed here [http://ha.ckers.org/webbug.html](http://ha.ckers.org/webbug.html)

Requests should be in the form `http://<server IP addres>/web-bug-server/index.php?id=<arbitrary document id>&type=<css|img>`


Example 1: Initializing the database
------------------------------------

You will need to initialize the database.  From your deployment directory
this is incredibly simple.  Just run the initialization script.

`$` **`cd /opt/sqlitebugserver`**

**`sudo python ./initialize.py`**

Note: This script will create the Sqlite database in the directory you're 
currently in.  It will create it with a weird long random name.  The reason
for this is to make it exceptionally difficult for an attacker to access your
data; without forcing users to go through any special configurations.

This script should set everything up for you.  Including linking the sqlitebugserver folder to your web root.

Example 2: Setting up the Web Bug Doc
-------------------------------------

First, you need to find the current IP address of your ADHD machine. Do
so by firing up a new terminal and using the ifconfig command.

`$` **`ifconfig`**

        eth0    Link encap:Ethernet  HWaddr 00:0c:29:6c:14:79
                inet addr:192.168.1.137  Bcast:192.168.1.255  Mask:255.255.255.0
                inet6 addr: fe80::20c:29ff:fe6c:1479/64 Scope:Link
                UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
                RX packets:117282 errors:0 dropped:0 overruns:0 frame:0
                TX packets:43840 errors:0 dropped:0 overruns:0 carrier:0
                collisions:0 txqueuelen:1000
                RX bytes:105331151 (105.3 MB)  TX bytes:4364108 (4.3 MB)
                Interrupt:19 Base address:0x2000

The IP address in this example is 192.168.1.137 so this is what will be
used from now on. Replace this with your own IP address wherever you see it appear.

Now you'll need to configure the web bug document to connect back to the
sqlite bug server running on your ADHD machine. Change into
/opt/sqlitebugserver/ and use the following command to edit the provided
.doc file. Replace 192.168.1.137 in the command with the IP address you found above.

`/opt/sqlitebugserver$` **`sed -r 's://.*/sqlitebugserver://192.168.1.137/sqlitebugserver:g' web_bug.html > web_bug.doc`**

Next, you will need to move web\_bug.doc to another machine. You can use
Linux (LibreOffice), Windows (Microsoft Word), or Mac OS (Microsoft Word
or TextEdit) to open the file. If you do not have another computer with
one of those applications installed, you can open it locally on the ADHD
machine. To get web\_bug.doc to another machine first copy it to the web directory.

`/opt/sqlitebugserver$` **`sudo cp web_bug.doc /var/www/`**

Then on the computer you want to copy the file to, open a web browser
and go to `http://192.168.1.137/web_bug.doc` to
download the document. Remember to replace the IP address with the one
you found for your local ADHD machine. Once the document is saved to the
remote computer, open it in one of the editors mentioned above to
trigger the bugs. See [Example 3: Viewing Bug Connections in the Database] on viewing the results.

NOTE: This assumings you have a web server running as you should on your default ADHD installation.

Example 3: Viewing Bug Connections in the Database
--------------------------------------------------

Any time a web bug is triggered, it makes a connection back to the
server running on the ADHD server, which then records information about
the connection in a database. Again, the only difference between SqliteBugServer
and WebBugServer is that SqliteBugServer is portable.  It doesn't use a
backend served database.  It simply uses an sqlite db.  A static file 
on the system.

You can view the data in the database by going to your deployment directory.
Then open the database by running: **`sqlite3 <database_name>.db`**

NOTE: As discussed below, there is a macro you can use to quickly open the db in the latest versions of the sqlitebugserver.  Simply run **`./opendb.py`**

Once you are inside the database simply send the command "select * from requests;"

From here, you can view all the entries in the database created by web
bugs. Each entry includes the document id which you can change by
editing the .doc file, the type of media request that was triggered, the
IP address the connection came from, and the time the connection was
made. The time is stored as a UNIX timestamp represented by the number
of seconds elapsed since 1 January 1970. There are numerous converters
available online that you can use to translate these into your local
time.

NOTE: If you have trouble figuring out the name of your db you can easily find it by running this command from your sqlitebugserver directory: **`ls -alh | grep db`**  If you wanted to be even more fancy this command could be used to open any db by name without having to know the name. **`sqlite3 $(ls -alh | awk '(/.db/) {print $9}')`** Just make sure to run it from the right folder.  In the latest versions of sqlitebugserver you can just use the script **`./opendb.py`** to do all this for you.
