
Creepy
=======

Website
-------

<http://www.geocreepy.com/>

Description
-----------

Creepy is a geolocation and OSINT (open source intelligence) tool.

Install Location
----------------

`/opt/creepy/`

Usage
-----

To launch Creepy you must first make sure that all the necessary dependencies have been installed
The adhd build script should have already taken care of this, but if it has not you can find a list
of dependencies on creepy's website <http://www.geocreepy.com/>


Once the dependencies are installed simpy cd to the correct folder

`~$` **`cd /opt/creepy/creepy`**

And run the main application

`/opt/creepy/creepy$` **`python ./CreepyMain.py`**

`[recon-ng][default] >` **`use pushpin`**

This will launch the main application.


![Main Window](Creepy_files/creepy_0.jpg)

Example 1: Find Tweets Sent from Times Square
---------------------------------------------

Open a new terminal, change into the recon-ng directory and launch recon-ng:

`/opt/recon-ng$` **`./recon-ng`**

Twitter requires an API key to process our requests.
To get an API key you will need to first create a new app.

Visit https://dev.twitter.com/apps and create a new app.

As soon as the app is created you should be able to access the key and secret key.

These can be entered into the recon-ng database from the recon-ng prompt as follows:

`[recon-ng][default] >` **`keys add twitter_api XXXXXXXXX`**

and

`[recon-ng][default] >` **`keys add twitter_secret XXXXXXXXXXXXXXXXXXXXXX`**

You should only have to enter these once.

Now lets add a location record into the database to target our desired location.

Run these commands to add a location record pointed at Times Square:

`[recon-ng][default] >` **`query insert into locations (latitude, longitude) values ("40.758871","-73.985132");`**

Now that the fun part is out of the way, let's load up the twitter module.

`[recon-ng][default] >` **`use pins/twitter`**

Notice how we used the shorthand version of "recon/locations-pushpins/twitter" to load the module faster.

`[recon-ng][default][twitter] >` **`show options`**

         Name       Current Value  Req  Description
        ---------  -------------   ---  -----------
        RADIUS     1               yes  radius in kilometers
        SOURCE     default         yes  source of input (see 'show info' for details)

Looks like everything is good to go, the radius is set, the SOURCE should be automatically reading from the database where our location is stored.
Now let's run the module.

`[recon-ng][default][twitter] >` **`run`**

        --------------------
        40.758871,-73.985132
        --------------------
        [*] Collecting data for an unknown number of tweets...
        [*] 1349 tweets processed.
        
        -------
        SUMMARY
        -------
        [*] 862 total (862 new) items found.


Once the module completes execution the data has been saved to the database.
To view it, we will need to use the reporting module.

`[recon-ng][default][twitter] >` **`use reporting/pushpin`**

It's always a good idea to show the options before executing a module

`[recon-ng][default][twitter] >` **`show options`**

        Name            Current Value                  Req  Description
        --------------  -------------                  ---  -----------
        LATITUDE                                       yes  latitude of the epicenter
        LONGITUDE                                      yes  longitude of the epicenter
        MAP_FILENAME    /home/adhd/pushpin_map.html    yes  path and filename for pushpin map report
        MEDIA_FILENAME  /home/adhd/pushpin_media.html  yes  path and filename for pushpin media report
        RADIUS                                         yes  radius from the epicenter in kilometers

We will have to set the LATITUDE and LONGITUDE and RADIUS values real quick.

`[recon-ng][default][twitter] >` **`set LATITUDE 40.758871`**

`[recon-ng][default][twitter] >` **`set LONGITUDE -73.985132`**

`[recon-ng][default][twitter] >` **`set RADIUS 1`**

`[recon-ng][default][twitter] >` **`show options`**

        Name            Current Value                  Req  Description
        --------------  -------------                  ---  -----------
        LATITUDE        40.758871                      yes  latitude of the epicenter
        LONGITUDE       -73.985132                     yes  longitude of the epicenter
        MAP_FILENAME    /home/adhd/pushpin_map.html    yes  path and filename for pushpin map report
        MEDIA_FILENAME  /home/adhd/pushpin_media.html  yes  path and filename for pushpin media report
        RADIUS          1                              yes  radius from the epicenter in kilometers

That's better, now let's run this module

`[recon-ng][default][twitter] >` **`run`**

        [*] Media data written to '/home/adhd/pushpin_media.html'
        [*] Mapping data written to '/home/adhd/pushpin_map.html'

Once the module executes, it should automatically open firefox to your report.

![](Pushpin_files/image001.png)

**Page displaying all the tweets found within the specified border.**

![](Pushpin_files/image002.png)

**Map showing where each tweet came from. Clicking on a bubble shows the tweet.**

It's very important to note that gathering and reporting are two separate operations.
They can be set focus on differing geographical locations.  

Gathering loads information into the database.

Reporting takes information from the database and prepares it for human consumption.


