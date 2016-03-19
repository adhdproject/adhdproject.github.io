Dionaea
=======

Website
-------

<http://dionaea.carnivore.it/>

Description
-----------

Dionaea's intention is to trap malware exploiting vulnerabilities exposed by 
services offerd to a network, the ultimate goal is gaining a copy of the malware. 

Video Walkthrough
-----------------

<video controls>
  <source src="Videos/1_550_Dionaea.mp4">
  <source src="https://onedrive.live.com/download.aspx?cid=8D6C4317A39E3D29&resid=8D6C4317A39E3D29%2155676&canary=">
 <p>Your browser does not support html5 video.</p>
</video>

Example 1: Usage
----------------

First, lets become root and start Dionaea

Please click the Terminal Icon:

![](Dionaea_files/image076.png)

Then become root and start Dionaea:

![](Dionaea_files/image078.png)

Note: The password is honeydrive

Next, lets start the logging for Dionaea:

![](Dionaea_files/image080.png)

Now, lets start up Metasploit in a separate terminal window:

Please click the Terminal Icon:

![](Dionaea_files/image082.png)

Then become root and start msfconsole:

![](Dionaea_files/image084.png)

Please remember it can take a while to start

Then, we will start the correct smb exploit for Metasploit, please
remember you IP address will be different. If you need to, please run
ifconfig in another window:

![](Dionaea_files/image086.png)

Then, exploit

![](Dionaea_files/image088.png)

Please note the exploit will fail. It does not matter, Dionea captured
the requests.

Next, lets take a look at what Dionea logged:

![](Dionaea_files/image090.png)

The important thing to take from this is we can not only detect an
attack, we can also detect the structure of it. This can be useful for
not only seeing attacks, but writing signatures for more advanced
attacks.
