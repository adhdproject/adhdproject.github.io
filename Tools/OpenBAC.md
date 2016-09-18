OpenBAC POC
=======

Website
-------

<https://github.com/prometheaninfosec/openbac>

Description
-----------

OpenBAC is an open source implementation of the Ball and Chain password storage algorithm.

If you're curious as to what Ball and Chain is, please watch this conference
talk describing it in full. 
<https://www.youtube.com/watch?v=GfyM8lFkjo8&feature=youtu.be>


Long story short, Ball and Chain ties authentication to you network itself using large collections of random data.  These large collections of random data are incredibly hard for an attacker to extract, making it next to impossible for him to ever perform and offline attack against your user's passwords.

Think of it, kind of like tying down the stuff in your backyard before a hurricane.  We assume the wind is coming.  We harden ourselves against it.  It doesn't matter if the attacker can gain access to your network.  Even with root level control over the stored passwords, he still can't crack them (if implemented correctly).


You can use this library, and it's utilities to implement Ball and Chain is your next application (web or otherwise).  Your users will thank you.

Install Location
----------------

`/opt/openbac/`

Usage
-----

This collection comes complete with a library, utilities, and documentation.  You can find the documentation in the *docs* folder.  Do read more in there if you're interested.  

The library (openbac.py) at this moment is only written up in Python.  This is the code you can import directly into your application.  However, if for some reason you don't write in Python 100% of the time, you can use the utility server.py to use the library in any language.

Let's get started.

Eample 1: Initialization
------------------------

The first thing you will need to do is initialize the "array".  That is, you first have to build a large collection of random data to tie authentication to.  For this we use the utility "interactive-generate.py".  It does everything for you, all you need to do is follow the prompts.

The only tricky part here is that you have to pick a "pointer length".  When you put in a value, interactive-generate will tell you what size of array that would generate.  Then you can decide if that's the value that you want or not.

So, the pointer length value is something you may need to remember.  Interactive-generate is supposed to modify the config file (openbac.conf) itself.  But if something goes wrong you'll need to manually fill it out.  But what really matters is the size of the array.  You'll want to make it as big as you reasonably can.  The security of the Ball and Chain algorithm comes from making the array too big to steal.  

You can read more about this in the project documentation.

**`cd /opt/openbac/`**

`/opt/openbac$` **`./interactive-generate.py`**


Just follow the prompts.

Example 2: Generate a Hash
--------------------------

If you're used to using hashing algorithms during application dev then you're probably used to something like this:

```python
import hashlib

h = hashlib.md5()
h.update(passwd)
hashedPasswd = h.hexdigest()
```

This process takes the user password, and produces a pseudo-random, deterministic output.  The idea being that you can use the value output by the function to authenticate the user later, but that when you store only this value, it makes it very hard to discover what the user originally put in as their plaintext password, should the stored value (the hexdigest) be discovered.

Now, experience has shown us that time and again, things don't turn out that well.  Hashing algorithms fail again and again to secure the passwords stored via them.  Data breach after data breash shows us this.

That's why we have Ball and Chain.  For more information on the philosophy behind the algorithm you can read through the docs or contact the inventor on Twitter [@zaeyx](https://twitter.com/zaeyx).

Down to business...

With Ball and Chain, we do something completely different in the background.  But the output looks just about the same.  And when it comes to how you write your Ball and Chain enabled application, not that much is going to change.

For this tutorial, we're going to assume you're *not* writing in Python, and just show you how to use server.py.

First you may need to mess with the configuration file (openbac.conf).  Interactive-generate.py is supposed to change the settings it modified.  But if it doesn't you'll want to make sure they're set correctly.  The two settings to look out for are `arrayfile` and `pointerlengths`.  The configuration file is pretty heavily commented.  So it shouldn't be too hard to figure it out.  But seriously, if you need help.. Twitter [@zaeyx](https://twitter.com/zaeyx).

You need to set `pointerlengths` and `arrayfile` to be equal to the values you decided upon in interactive-generate.py.  

One final thing to keep in mind is that server.py works on a whitelist only basis.  It only accepts connections from hosts on its whitelist.  By default the loopback interface is the only whitelisted host (localhost).  If you need to run server.py on one machine, and connect to it from another you'll need to add that machine to the whitelist.

Once everything is configured.  Simply run the server like so:
`/opt/openbac$` **`sudo ./server.py`**

		#################
		Listening...


Now that the server is listening we can interact with it over http either in a web browser (for testing) or from your application (for production).

For this example, we are going to demonstrate a simple API call using the openssl s_client.  By default server.py only serves connections via the HTTP protcol wrapped with SSL (HTTPS).  This ensures that anyone passively listening to our communications cannot read anything.

NOTE: in production you will need to either use your own certifiate, or find a way to verify server.py's self-signed cert.  If you do not, an attacker may be able to perform a Man in The Middle attack and steal your communication.

The API call we will make takes the user's password as input, and returns a hash value as output.

To call it we simply need to perform a get request for /?mk=<user's_passwd>

By default the server should be listening on localhost:2337.  So we will connect like so.

`/opt/openbac$` **`sudo openssl s_client -connect localhost:2337`**

		SSL-Session:
   		    Protocol  : TLSv1.2
		    Cipher    : ECDHE-RSA-AES256-GCM-SHA384
		    Session-ID: FF168906A48DF98DE0DC0EA3167EF8A708F16E640C3C68659D316CE4FFD419F1
		    Session-ID-ctx:
		    Master-Key: ACCF1C7D9397912F31292E556FCA72560BFC9D95541A09E591DE9F1537D67E169A2D5AA4FD8A269584BDBD6041B82945
		    Key-Arg   : None
		    PSK identity: None
		    PSK identity hint: None
		    SRP username: None
		    TLS session ticket lifetime hint: 300 (seconds)
		    TLS session ticket:
		    0000 - 6e c1 b3 cb 18 74 43 19-32 27 d5 b3 ad 51 89 3b   n....tC.2'...Q.;
		    0010 - d2 8e 6f 2c db 2b a3 dc-36 3b d5 bd 2c 71 fa 38   ..o,.+..6;..,q.8
		    0020 - 17 1d 86 5a 2a 35 70 2b-b2 67 3a 60 07 df bf 29   ...Z*5p+.g:`...)
		    0030 - 8a 58 51 ee 46 bb fe 94-ea af 24 a6 6c 73 8b cb   .XQ.F.....$.ls..
		    0040 - a4 41 28 4e 3a 66 4b 03-9b ef 93 eb 9f e3 99 3c   .A(N:fK........<
		    0050 - a1 99 18 24 25 50 7a 61-c7 e1 4a e0 aa d4 11 7b   ...$%Pza..J....{
		    0060 - 3c e7 38 80 20 e4 70 1f-ef d4 ff 8e db ae 37 95   <.8. .p.......7.
		    0070 - 2f f3 b7 c6 4e 6b 4d 60-ce ed 54 e6 72 f1 b5 42   /...NkM`..T.r..B
		    0080 - 0c 64 9b 1e 7d a6 13 f4-af 6a 92 9b e4 67 78 f0   .d..}....j...gx.
		    0090 - c1 e2 70 4c d2 26 51 82-23 ec e9 c5 48 2b a6 f4   ..pL.&Q.#...H+..
		
		    Start Time: 1474156923
		    Timeout   : 300 (sec)
		    Verify return code: 18 (self signed certificate)
		---

	

You will see a ton of data that looks like this fly by.  Don't worry about it.  What you're seeing here is information about the SSL connection.  Your cursor should be sitting right below the three dashes, waiting for input.   Type this in: `GET /?mk=mypasswd` and hit enter twice.

The server should spit back something like this:

		d4f05105d7eb89e00cb342eea5e56915:::fa8bba56e5ad79017ef9856725ee2e952f49320dc1e71daed4350ca12618ce09fd67c859af4e2f341a965a37f0c2read:errno=0

Throw out the last little bit "read:errno=0" (an artifact of the way we connected) and you have your hash.

Store this hash somewhere.  We're going to use it in a second.

Example 3: Authentication
-------------------------

There are two calls to the server.py API that can help you perform authentication.  The first one takes the hash, and the user's password and unpacks the hash before making the requisite checks to determine whether or not the correct password was supplied.  The second one takes an already unpacked hash and just performs the checks against the array.  (Again, for now don't worry too much about the cryptographic magic behind the scenes.  Check out the documentation if you want to know more.)

The three API calls we care about with server.py are:

* ?mk=<userpass> ==> Generate a hash with this password.
* ?up=<userhash>&passwd=<userpass> ==> Check if user provided right pass
* ?au=<unpackedhash> ==> Check if hash plaintext is valid.

We used `mk` before.  Now we're going to use `up`.  We will need the hash we generated in [Example 2: Generate a Hash].  We will also need the password we used to generate that hash.  If you don't have both of those things, go back and run through the generation process again.

Let's get started.  We will connect in the same way we did before.

`/opt/openbac$` **`sudo openssl s_client -connect localhost:2337`**

		 SSL-Session:
                    Protocol  : TLSv1.2
                    Cipher    : ECDHE-RSA-AES256-GCM-SHA384
                    Session-ID: FF168906A48DF98DE0DC0EA3167EF8A708F16E640C3C68659D316CE4FFD419F1
                    Session-ID-ctx:
                    Master-Key: ACCF1C7D9397912F31292E556FCA72560BFC9D95541A09E591DE9F1537D67E169A2D5AA4FD8A269584BDBD6041B82945
                    Key-Arg   : None
                    PSK identity: None
                    PSK identity hint: None
                    SRP username: None
                    TLS session ticket lifetime hint: 300 (seconds)
                    TLS session ticket:
                    0000 - 6e c1 b3 cb 18 74 43 19-32 27 d5 b3 ad 51 89 3b   n....tC.2'...Q.;
                    0010 - d2 8e 6f 2c db 2b a3 dc-36 3b d5 bd 2c 71 fa 38   ..o,.+..6;..,q.8
                    0020 - 17 1d 86 5a 2a 35 70 2b-b2 67 3a 60 07 df bf 29   ...Z*5p+.g:`...)
                    0030 - 8a 58 51 ee 46 bb fe 94-ea af 24 a6 6c 73 8b cb   .XQ.F.....$.ls..
                    0040 - a4 41 28 4e 3a 66 4b 03-9b ef 93 eb 9f e3 99 3c   .A(N:fK........<
                    0050 - a1 99 18 24 25 50 7a 61-c7 e1 4a e0 aa d4 11 7b   ...$%Pza..J....{
                    0060 - 3c e7 38 80 20 e4 70 1f-ef d4 ff 8e db ae 37 95   <.8. .p.......7.
                    0070 - 2f f3 b7 c6 4e 6b 4d 60-ce ed 54 e6 72 f1 b5 42   /...NkM`..T.r..B
                    0080 - 0c 64 9b 1e 7d a6 13 f4-af 6a 92 9b e4 67 78 f0   .d..}....j...gx.
                    0090 - c1 e2 70 4c d2 26 51 82-23 ec e9 c5 48 2b a6 f4   ..pL.&Q.#...H+..

                    Start Time: 1474156923
                    Timeout   : 300 (sec)
                    Verify return code: 18 (self signed certificate)
		---

Now we need to make the call to the unpack function.  You'll need the password and hash here.  Don't forget to hit enter twice once you're done.

`GET /?up=d4f05105d7eb89e00cb342eea5e56915:::fa8bba56e5ad79017ef9856725ee2e952f49320dc1e71daed4350ca12618ce09fd67c859af4e2f341a965a37f0c2&passwd=mypasswd`

If everything checks out, this should return *True*.  Otherwise it will return *False*.  Assuming that in your actual code, there wouldn't be any fear of typos or anything, the deciding factor would be whether or not the user entered the correct password.

If the function returns *True* you would authenticate the user.  It's that simple.


Example 4: Implementing in Code
-------------------------------

So, how would you actually implement this in your code?  Remember, if your application is written partially or solely in Python, you can use the python library openbac.py.  But assuming it is not, you can instead configure server.py to run and accept connections from the server your application runs on.  Then you can simply make calls to it via HTTPS just like we did in the examples prior.

Here are some bits of code that you could use in a few different languages.

**PHP:**

```php
		$username = $_POST['user_name'];
		$passwd = $_POST['user_passwd'];
		$hash = lookup_hash($username);
		
		$auth_result = file_get_contents("https://localhost:2337/?up=".$hash."&passwd=".$passwd);
		
```

**Python:**

```python
		import urllib2

		username = raw_input("Enter your username: ")
		passwd = raw_input("Enter your passwd: ")
		hash = lookup_hash(username)

		auth_result = urllib2.urlopen("https://localhost:2337/?up=%s&passwd=%s" % (hash, passwd))
```

**Jquery-Ajax:**

```javascript
		$.get("https://localhost:2337/?mk="+hash+"&passwd="+passwd
		, function(data, status){
			alert("Result: " + data);
		});

```

You get the idea.

Just set the server up to run.  Whitelist the address the connections will be coming from.  Then make simple HTTPS requests from your application.
