Lockdown
=======

Website
-------

<https://github.com/PrometheanInfoSec/Lockdown>

Description
-----------

Lockdown is your digital saferoom.  It was born of a realization that 
there is very little someone can quickly do to secure their digital life
if they are compromised.  For example, if someone stole your phone?  What could
you do to keep them out of your email accounts?

Lockdown exists to respond to this need.  Configure Lockdown to be able to access your web accounts.  
Configure it to listen on an email address.
Then set the script to run 24/7 on a server of your choosing.  If something bad happens
simply email the panic password to the email lockdown is listening on.  Lockdown 
will log into your linked accounts, and change your passwords in a manner of seconds.

So now, if your phone is stolen you can secure your accounts as fast as you can
find a way to send an email.  (Grab a passerby's phone, might be done in under a minute).

Install Location
----------------

`/opt/lockdown/`

Usage
-----


Running Lockdown is easy, cd to the correct directory.

`~$` **`cd /opt/lockdown`**

And run the application

`/opt/lockdown$` **`python ./lockdown.py`**

As long as the script is configued correctly, it should now be listening
for the panic email, ready to protect you!

Note that this script requires access to a gmail account. Some accounts will restrict this and Lockdown won't be able to listen for the failsafe email. To remove this restriction, log into the listening gmail account, go to [https://www.google.com/settings/security/lesssecureapps](https://www.google.com/settings/security/lesssecureapps), and 'Turn On' access for less secure apps.

Example 1: Customizing the Configurations
-----------------------------------------
You will need to set the configurations properly in order to actually run this 
script.  The setup is quite straight forward.  The comments in the file
should explain everything you need to know.  

Here is what you need to configure.
mail_user --> Username for gmail account to listen with.
mail_pass --> Password for said account.
panic_phrase --> secret phrase that will trigger the failsafe
creds['twitter'] --> Twitter account credentials.  See Comment in code for format.
creds['facebook'] --> Facebook account credentials.  See comment in code for format.
creds['google'] --> Google account credentials.  See comment in code for format.
new_password --> Password to change all accounts to in emergency.

Set all these variables to your liking, and run the script.  

`/opt/lockdown$` **`python ./lockdown.py`**

It should now be listening.

Example 2: Triggering the failsafe
----------------------------------

In the event of an emergency, as long as everything is configured correctly
and actively running, all you need to do to trigger the failsafe is email the 
panic phrase you set to the email address you configured the script to listen on with the subject line "panic" (not case sensitive). A browser will open and it will automatically change the passwords in front of your eyes.

It's that easy.
