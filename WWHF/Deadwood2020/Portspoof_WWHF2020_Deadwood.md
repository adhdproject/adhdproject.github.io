Example 1: WWHF 2020 Ctf Portspoof
----------------------
Before you do this lab, I would reccomend heading over to the Portspoof usage doc included in
adhdproject.github.io and take a look at that. If you're super awesome at this stuf or you already looked at it, carry on! :D

Let's make sure we are ready to run Portspoof:
`~$` **`sudo su -`**
`~#` **`iptables -F`**
`~#` **`iptables -t nat -A PREROUTING -p tcp -m tcp --dport 1:65535 -j REDIRECT --to-ports 4444`**

First, we are going to take a look at the service signature file included with ADHD4.
`~#` **`nano /usr/local/etc/portspoof_signatures`**
We aren't going to do anything crazy with it, but you can see that this is an intense list of service signatures. Scroll far enough and you will see a signature for Battlefield 2 in there.

Portspoof allows us to create custom signature files as well. For example, we could create a file with just an ftp signature in it. To do this, just create a new file named **`test`** and place it into **`/usr/local/etc`** Then add the following line to it and save the file: **`220 ([-.+\w]+) FTP server \(Version (\d[-.\w]+)\([^\)]+\) [A-Z][a-z][a-z] [A-Z].*200\d\) ready\.\r\n`**

Now let's startup Portspoof and tell it to use our new signature file
`~#` **`portspoof -s /usr/local/etc/test`**

Now let's scan our ADHD vm with Nmap. From another machine that has Nmap installed and can network to the ADHD vm run: `~#` **`nmap -F -sV YOUR_ADHD_IP`** You can get the IP of your ADHD vm by using **`ifconfig`** in a terminal on your ADHD vm.  Once the scan is done, you should see output in your Nmap terminal showing a lot of ports that appear to have an FTP server running on them. However, if we try to connect to one we will not be succesful since the port is not actually open.

Finally, there is one more service signature file located at **`/usr/local/etc/wwhf_sigs`** Try giving it to Portspoof and using Nmap to find the flags for the CTf One is easy, the other might not be!

When done, clean up your ADHD vm with `~#` **`iptables -F`** and a reboot.