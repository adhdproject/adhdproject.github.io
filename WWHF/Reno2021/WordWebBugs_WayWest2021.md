WWHF 2021 Word Web Bugs
----------------------
Before you do this lab, I would recommend taking a look at the Web Bug Server tool usage documentation located at adhdproject.github.io This lab will focus on possible use cases for Web Bug Server and not so much on the details of how to use the tool. If you're ready, lets dive in!

Explore the db
------------------
For this lab, we are going to pretend that we are managing a network for a Widget company. There have been suspicions of an employee leaking important information from the company, so we have decided to drop some bugged documents with enticing names in some easy to access places. It is important to note that word web bug has the ability to create a unique id for each bug. We can  change it for each bug by editing the html that gets embedded into each doc. Suppose we have three suspects, Tim, Jim, and Slim.  We want a separate id for each suspect, so let's say our ids will be 84, 74, and 83, respectively.   Time to drop the bugs and see who is the leak. One thing to note before we look at the db: sending a bugged doc to someone via email will probably cause a bit of noise from email providers following links.

Let's login to the database and see if we have any hits! Browse to http://127.0.0.1/adminer and login using `127.0.0.1`, `webbuguser`, `adhd`, and `webbug` for the Server, Username, Password, and Database respectively. Once logged in, click on the `requests` table, and then click `Select data`.

We have a lot of hits! It's important to remember that a handful of triggers for each bug is probably normal, especially if we consider the user agent and ip address. Triggers with an ip address that comes from our own internal network are not as concerning. With that in mind, we can easily rule out Tim from being the suspect. His bug was only triggered 4 times. and all from inside our internal network at the office. 

Jim also probably isn't the suspect, since his bug was only triggered twice. It looks like he was on the vpn though; maybe he's accessing company material on a personal device at home? The user agent for Jim also looks strange, maybe it's worth investigating further! 

That leaves only Slim to look at, and with 10 different hits it definitely looks like he's the culprit. Let's look at some of the external ip addresses and see if we can find any information about them! That 104.23.99.190 ip address keeps popping up a lot. Give it a google and see if you can find solid evidence that Slim is the culprit! 

Other Uses
----------------
It is important to realize that the scenario described above is not the only way to use web bug server. Since the functionality boils down to http requests made to a listening server, we could employ web bug server to alert us if a website is being scanned. Simply embed the 1x1 pixel image somewhere on the home page and link it to the listening server. A normal user would never find that link, but a web crawler sure would! 

Cleanup
---
Probably not needed after this lab, but if things are wonky reboot your ADHD vm to be safe.
