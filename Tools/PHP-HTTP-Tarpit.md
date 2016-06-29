PHP-HTTP-Tarpit
=======

Website
-------

<https://github.com/msigley/PHP-HTTP-Tarpit>

Description
-----------

PHP-HTTP-Tarpit is a tool designed to confuse and trap misbeheaving webspiders.  It accomplishes this task through a combination of log fuzzing, and error spoofing.


Install Location
----------------

`/opt/PHP-HTTP-Tarpit/`


Usage
---------------------

Here's what you need to do in order to deploy the tarpit on your webapp.  First you need to copy the file la_brea.php into a folder accessible to the clients of your web application.  Next, Simply include a hidden reference to this page inside some portion of your application.  This reference should be hidden so that no users accidentally stumble onto it.  Something like a hidden link would be perfect.  Something a web spider would want to check out.

Once you've done these two steps.  You're golden.  

Example 1: Deployment
------------------------

To get started, for our example we will copy the tarpit into the web root of out web application.

`$` **`cp /opt/PHP-HTTP-Tarpit/la_brea.php /var/www/`**

Next we want to insert a reference into some portion of your web application that points to this file.  For example, we might edit your application's index.php appending this line:

		<a href="/la_brea.php" ></a>

It's that easy.
