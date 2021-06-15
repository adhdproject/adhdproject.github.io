WWHF 2020 ADHD Labs PSAD
----------------------
PSAD is an intrusion detection and log analysis tool that runs on the backbone that is iptables. If you haven't seen or ran PSAD before, make sure to check out the ADHD documentation for the tool here: adhdproject.github.io

Warning: For this lab it is important that you do not start PSAD. This will overwrite the log files and any interesting bits may be lost. A copy of the log file can be found at /root

Thanks to Canary Tokens, we know that there is something funny going on within our network. Fortunately, we had PSAD running on the same host as Canary. Around these parts we like to protect our bait. Also thanks to Canary, we know the honey pot was triggered from our internal 10.10 network and an external 10.70 network.

Lets take a look at the logs and see if PSAD detected anything. The file is at **`/var/log/psad/status.out`** Let's cat this and take a look. **`sudo cat /var/log/psad/status.out`** 
First, scroll to the top of the output and peruse the top 50 signatures detected. We immediately see a lot of BACKDOOR flags. That can't be good! 

Now scroll down until you see a line that says `IP Status Detail` It looks like PSAD logged traffic from two different hosts: `10.10.174.182` and `10.70.0.4` That first host looks familiar! It's the host that triggered our Canary token. It looks like this host ran an nmap scan. 

The second host appears to be up to far more nefarious deeds! It also looks like this host came from the same 10.70 network! Based on the traffic from this host, it might be a good idea to add it to the deny list! As for the internal host, lets check with the systems admin team and make sure it hasn't been compromised!



