
Honey Ports
============

Website
-------

<https://code.google.com/p/honeyports/>

Description
-----------

A Python based cross-platform HoneyPort solution, created by Paul Asadoorian.

Install Location
----------------

`/opt/honeyports/cross-platform/honeyports/`

Usage
-----

Change to the Honeyports directory and execute the latest version of the script:

`~$` **`cd /opt/honeyports/cross-platform/honeyports`**

`/opt/honeyports/cross-platform/honeyports$` **`python2 ./honeyports-0.4a.py`**

        Usage: honeyports-0.4a.py -p port 
        Please specify a valid port range (1-65535) using the -p option

Video Walkthrough
-----------------

### Part 1

<video controls>
  <source src="Videos/1_550_HoneyPorts_1.mp4">
  <source src="https://onedrive.live.com/download.aspx?cid=8D6C4317A39E3D29&resid=8D6C4317A39E3D29%2155672&canary=">
 <p>Your browser does not support html5 video.</p>
</video>

### Part 2

<video controls>
  <source src="Videos/1_550_HoneyPorts_2.mp4">
  <source src="https://onedrive.live.com/download.aspx?cid=8D6C4317A39E3D29&resid=8D6C4317A39E3D29%2155671&canary=">
 <p>Your browser does not support html5 video.</p>
</video>

Example 1: Monitoring A Port With HoneyPorts
--------------------------------------------

From the honeyports directory, run:

`/opt/honeyports/cross-platform/honeyports$` **`sudo python2 ./honeyports-0.4a.py -p 3389`**

        Listening on  0.0.0.0 IP:  0.0.0.0  :  3389

We can confirm that the listening is taking place with lsof:

`/opt/honeyports/cross-platform/honeyports$` **`sudo lsof -i -P | grep python`**

        python   26560     root    3r  IPv4 493595      0t0  TCP *:3389 (LISTEN)

Looks like we're good.

Any connection attempts to that port will result in an instant ban for the IP address in question.
Let's simulate this next.

Example 2: Blacklisting In Action
---------------------------------

If Honeyports is not listening on 3389 please follow the instructions in 
[Example 1: Monitoring A Port With HoneyPorts].

Once you have Honeyports online and a backup Windows machine to connect to Honeyports from, 
let's proceed.

First we need to get the IP address of the ADHD instance.  

`~$` **`ifconfig`**

        eth0      Link encap:Ethernet  HWaddr 08:00:27:65:3c:64  
                  inet addr:192.168.1.109  Bcast:192.168.1.255  Mask:255.255.255.0
                  inet6 addr: fe80::a00:27ff:fe65:3c64/64 Scope:Link
                  UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
                  RX packets:46622 errors:0 dropped:0 overruns:0 frame:0
                  TX packets:8298 errors:0 dropped:0 overruns:0 carrier:0
                  collisions:0 txqueuelen:1000 
                  RX bytes:14057203 (14.0 MB)  TX bytes:2659309 (2.6 MB)

        lo        Link encap:Local Loopback  
                  inet addr:127.0.0.1  Mask:255.0.0.0
                  inet6 addr: ::1/128 Scope:Host
                  UP LOOPBACK RUNNING  MTU:16436  Metric:1
                  RX packets:94405 errors:0 dropped:0 overruns:0 frame:0
                   TX packets:94405 errors:0 dropped:0 overruns:0 carrier:0
                     collisions:0 txqueuelen:0 
                  RX bytes:37127292 (37.1 MB)  TX bytes:37127292 (37.1 MB)
We can see from the ifconfig output that my ADHD instance has an IP of 192.168.1.109

I will connect to that IP on port 3389 from a box on the same network segment in order to test 
the functionality of Honeyports.

I will be using RDP to make the connection.

To open Remote Desktop hit `Windows Key + R` and input `mstsc.exe` before hitting OK.

![](HoneyPorts_files/Image_001.png)

Next simply tell RDP to connect to your machine's IP address.

![](HoneyPorts_files/Image_002.png)

We get an almost immediate error, this is a great sign that Honeyports is doing its job.

![](HoneyPorts_files/Image_003.png)

Any subsequent connection attempts are met with failure.

![](HoneyPorts_files/Image_004.png)

And we can confirm back inside our ADHD instance that the IP was blocked.

`~$` **`sudo iptables -L`**

        Chain INPUT (policy ACCEPT)
        target     prot opt source               destination         
        REJECT     all  --  192.168.1.149        anywhere             reject-with icmp-port-unreachable

        Chain FORWARD (policy ACCEPT)
        target     prot opt source               destination         

        Chain OUTPUT (policy ACCEPT)
        target     prot opt source               destination         

        Chain ARTILLERY (0 references)
        target     prot opt source               destination    

You can clearly see the REJECT policy for 192.168.1.149 (The address I was connecting from).

To remove this rule we can either:

`~$` **`sudo iptables -D INPUT -s 192.168.1.149 -j REJECT`**

Or Flush all the rules:

`~$` **`sudo iptables -F`**

Example 3: Spoofing TCP Connect for Denial Of Service
-----------------------------------------------------

Honeyports are designed to only properly respond to and block full TCP connects.  This is done to 
make it difficult for an attacker to spoof being someone else and trick the Honeyport into blocking 
the spoofed address.  TCP connections are difficult to spoof if the communicating hosts properly 
implement secure (hard to guess) sequence numbers.  Of course, if the attacker can "become" the 
host they wish to spoof, there isn't much you can do to stop them.

This example will demonstrate how to spoof a TCP connect as someone else, for the purposes of 
helping you learn to recognize the limitations of Honeyports.

If you can convince the host running Honeyports that you are the target machine, you can send 
packets as the target.  We will accomplish this through a MITM attack using ARP Spoofing.

Let's assume we have two different machines, they may be either physical or virtual.
One must be your ADHD machine running Honeyports, the other for this example will be a Kali box.
They must both be on the same subnet.

Note: Newer Linux operating systems like ADHD often have builtin protection against this attack. 
This protection mechanism is found in **/proc/sys/net/ipv4/conf/all/arp_accept**. A **1** in this
file means that ADHD is configured to accept unsolicited ARP responses. This setting is already 
set for you, but you may verify it by inspecting this file.

If our ADHD machine (running the Honeyports) is at 192.168.1.144 and we want to spoof 192.168.1.1

Let's start by performing our MITM attack.

`~#` **`arpspoof -i eth0 -t 192.168.1.144 192.168.1.1 2>/dev/null &`**

`~#` **`arpspoof -i eth0 -t 192.168.1.1 192.168.1.144 2>/dev/null &`**

If you want to confirm that the MITM attack is working first find the MAC address of the Kali box.

`~#` **`ifconfig -a | head -n 1 | cut -d" " -f 11`**

        00:0c:29:40:1c:d3

Then on the ADHD machine run this command to determine the current mapping of IPs to MACs.

`~#` **`arp -a`**

Look to see if the IP you are attempting to spoof is mapped to the MAC address from the previous step.

Once we have properly performed our arpspoof we will move on to assigning a temporary IP to the 
Kali machine.

This will convince the Kali machine to send packets as the spoofed host.

`~#` **`ifconfig eth0:0 192.168.1.1 netmask 255.255.255.0 up`**

The last step is to connect from the Kali box to the ADHD machine on a Honeyport, as 192.168.1.1

For this example, lets say that port 1337 is a Honeyport.

`~#` **`nc 192.168.1.144 1337 -s 192.168.1.1`**

It's that easy, if you list the firewall rules of the ADHD machine you should find a rule rejecting 
connections from 192.168.1.1

Mitigation of this vulnerability can be accomplished with either MITM protections, or careful 
monitoring of the created firewall rules.


