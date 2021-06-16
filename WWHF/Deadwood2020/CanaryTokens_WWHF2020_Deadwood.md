Example 1: WWHF 2020 Canary Tokens
----------------------
Before you do this lab, I would reccomend heading over to the Canary Token Local Demo usage doc included in
adhdproject.github.io and take a look at that. If you're super awesome at this stuff or you already looked at it, carry on! :D

Let's make sure we are ready to run Canary Tokens:
`~$` **`cd /opt/canarytokens-docker`**
`~$` **`nano switchboard.env`**
Switchboard.env is a configuration file for canary. Find the line that says CANARY_PUBLIC_IP= and put your ip there. You can find your ip with **`ifconfig`** or **`ip addr`**.  
Next, we need to shutdown dnsmasq, apache, artilery, and master.
`~$` **`sudo killall dnsmasq`**
`~$` **`sudo service apache2 stop`**
`~$` **`sudo killall -9 master`**
To kill artillery, run this command to find the process id.
`~$` **`sudo netstat -planet | grep python`**
Then, kill the process.
`~$` **`sudo kill <process id from previous command>`**
Finally we can start Canary with this command:
`~$` **`sudo docker-compose up`**

Now that the service is up, we can browse to http://localhost/generate
You should see the default Canary interface. From here, we can create and manage new tokens. Feel free to play around with the different kinds of tokens. For this lab we are going to take a look at a token that has been already deployed.

For this lab we are going to take a look at a previously deployed token and analyze the triggers. An annoying feature of the Canary interface is that it doesn't include a good menu to interact with previous tokens. This means we need to save the management link for each token we deploy so we can get back to it.
Copy this link and paste it into your firefox: http://localhost/manage?token=v4pi8p90bsx3ok269lkjenlp1&auth=05425f171b378795719fc584bcefafac
 
You should be greeted with a Canary token that has been triggered several times. Using the information from the triggers, we can deduce some more information about what might have caused this to happen.
First, we can note that the token was triggered from two different networks. Let's assume that the 10.10 network is our internal network. Notice anything odd about the token? It was triggered 6 times from that network. What might this tell us? Perhaps someone was doing some aggressive scanning internally. Or, maybe all of those triggers were from Bobby the intern testing the functionality. 

The more concerning trigger is from the 10.70 network. Worst case scenario, someone was able to access our internal space from an external connection. That could be bad. Or, maybe this token was accidentally copy pastad into a public facing service. Dang interns anyway. Regardless of what triggered it, the fact that it has been triggered externally should sound the alarm and we should do some serious investigation!




When done, clean up your ADHD vm with a reboot.
