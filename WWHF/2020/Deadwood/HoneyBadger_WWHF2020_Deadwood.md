HoneyBadger
======

Introduction
------------
HoneyBadger is a tool that can very accurately geolocate machines that trigger
file-based or web-based tokens. Using the Google Maps API, geolocation is often
incredibly accurate, sometimes up to a few meters.

These four labs provide a brief look into HoneyBadger's features. To start
HoneyBadger, run the following commands:
```
cd /opt/honeybadger/server
python3 honeybadger.py
```

Then, navigate to http://localhost:5000 via Firefox and log in with
`adhd`:`adhd`.

**NOTE**: while playing with the WWHF ADHD image, don't reinitialize the database.
This will break the CTF challenges, which have been hand crafted and will be
lost if discarded. If you wish to play with HoneyBadger outside of the CTF,
feel free to download it using the link in the official documentation, or
download a regular ADHD4 image.

Lab 1
-----
Log in to HoneyBadger and familiarize yourself with the interface's different
tabs.

One of the target names should catch your attention.

Lab 2
-----
Looks like a client attempted to beacon in, but used an invalid target GUID.
It happened very early in this server's lifetime.

What GUID did the client unsuccessfully attempt to beacon to?

Lab 3
-----
Check out the `map` tab. Even without a Google Maps API key, the map should
still load and let you view the map, with a developer watermark.

Perhaps there's a CTF flag inside a beacon associated with the target you
looked at in lab 1.

Lab 4
-----
This lab is more elaborate (and hopefully more challenging) than the previous.

You may have noticed another target in the `target` tab that we haven't looked
at yet. There are several beacons associated with this target.

There are many unknowns about this target. The name of the target is somewhat
strange, and the beacons seem to have some sort of order. What could that mean?
Why would the accuracy value be 0? Does that make the coordinates pointless?
