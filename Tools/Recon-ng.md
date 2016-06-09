Recon-ng
============

Website
-------

<https://bitbucket.org/LaNMaSteR53>

Description
-----------

Recon-ng, the definitive framework for passive reconnaissance.  Developed by Tim Tomes (Aka LaNMaSteR53).

Install Location
----------------

`/opt/recon-ng/`

Usage
-----

Recon-ng's UI works in a similar way to the Metasploit framework's.

To run Recon-ng first change into the install directory:

`~$` **`cd /opt/recon-ng`**

And run the main application with the `--no-check` flag to tell Recon-ng to not check for updates:

`/opt/recon-ng$` **`./recon-ng --no-check`**

Let's take a look at the available commands.

`[recon-ng][default] >` **`help`**

        Commands (type [help|?] <topic>):
        ---------------------------------
        add             Adds records to the database
        back            Exits current prompt level
        del             Deletes records from the database
        exit            Exits current prompt level
        help            Displays this menu
        keys            Manages framework API keys
        load            Loads selected module
        pdb             Starts a Python Debugger session
        query           Queries the database
        record          Records commands to a resource file
        reload          Reloads all modules
        resource        Executes commands from a resource file
        search          Searches available modules
        set             Sets module options
        shell           Executes shell commands
        show            Shows various framework items
        spool           Spools output to a file
        unset           Unsets module options
        use             Loads selected module
        workspaces      Manages workspaces

To show global options run:

`[recon-ng][default] >` **`show options`**

        Name          Current Value  Req  Description
        ------------  -------------  ---  -----------
        DEBUG         False          yes  enable debugging output
        NAMESERVER    8.8.8.8        yes  nameserver for DNS interrogation
        PROXY                        no   proxy server (address:port)
        STORE_TABLES  True           yes  store module generated tables
        TIMEOUT       10             yes  socket timeout (seconds)
        USER-AGENT    Recon-ng/v4    yes  user-agent string
        VERBOSE       True           yes  enable verbose output

To set a global option:

`[recon-ng][default] >` **`set USER-AGENT Google Chrome`**

        USER-AGENT => Google Chrome

Globally set options are set in all modules.

Example 1: Finding Hosts With google_site_web
---------------------------------------------

Let's take a look at how a Recon-ng module is used.

If you haven't already started recon-ng execute:

`~$` **`/opt/recon-ng/recon-ng --no-check`**

From the main menu run:

`[recon-ng][default] >` **`use google_site_web`**

Recon-ng has smart module selection, so you don't need the full path to the module.  
If you have a unique string, recon-ng will select the associated module. If your string 
matches multiple modules, Recon-ng will present them to you.

For example:

`[recon-ng][default][google_site_web] >` **`use google`**

        [*] Multiple modules match 'google'.
        
        Recon
        -----
            recon/domains-hosts/google_site_api
            recon/domains-hosts/google_site_web


As you can see "google_site_web" is a string unique to that specific module, that is why it loaded.

Now that we have the module loaded up, let's show the options to see what we need to set before we can run.

It is always a good idea to show the options before attempting to execute a module.

`[recon-ng][default][google_site_web] >` **`show options`**

        Name    Current Value  Req  Description
        ------  -------------  ---  -----------
        SOURCE  default        yes  source of input (see 'show info' for details)

It looks like we only have one option to set.  

As in the description of the SOURCE option, to see more about the module let's run 'show info'

`[recon-ng][default][google_site_web] >` **`show info`**

        <<<Output has been truncated due to length>>>
        Source Options:
            default         SELECT DISTINCT domain FROM domains WHERE domain IS NOT NULL ORDER BY domain
            <string>        string representing a single input
            <path>          path to a file containing a list of inputs
            query <sql>     database query returning one column of inputs

As you can see, we can set the SOURCE to be a few different things.  For this example, 
we are going to be setting it to a single string.

To do that, run this command.

`[recon-ng][default][google_site_web] >` **`set source google.com`**

        SOURCE => google.com

Make sure you do not put `www` before your target domain.  This module is going to be searching 
for subdomains of the specified domain so will find `www` and any others on its own.

Next let's run the module:

`[recon-ng][default][google_site_web] >` **`run`**

      ----------
      GOOGLE.COM
      ----------
      [*] URL: http://www.google.com/search?start=0&filter=0&q=site%3Agoogle.com
      [*] classroom.google.com
      [*] news.google.com
      [*] www.google.com
      [*] helpouts.google.com
      [*] cloud.google.com
      [*] Sleeping to avoid lockout...
      
      -------
      SUMMARY
      -------
      [*] 5 total (2 new) items found.

That covers the basics of running a module in Recon-ng there are tons more available modules 
so don't stop here.  Next we will look at how to retrieve captured data.

Example 2: Viewing Stored Data
------------------------------

Recon-ng modules store data to the Recon-ng database.  This data can later be retrieved via 
built-in commands or direct access to the database.

From within Recon-ng run:

`[recon-ng][default] >` **`show schema`**

        +--------------------+
        |    repositories    |
        +--------------------+
        | name        | TEXT |
        | owner       | TEXT |
        | description | TEXT |
        | resource    | TEXT |
        | category    | TEXT |
        | url         | TEXT |
        | module      | TEXT |
        +--------------------+

That's an example of one of the schemas.

To show the database layout.

You can select all data from any of these tables with the show command, for example:

`[recon-ng][default] >` **`show hosts`**

        +----------------------------------------------------------------------------+
        |         host         | ip_address | latitude | longitude |      module     |
        +----------------------------------------------------------------------------+
        | classroom.google.com |            |          |           | google_site_web |
        | cloud.google.com     |            |          |           | google_site_web |
        | helpouts.google.com  |            |          |           | google_site_web |
        | news.google.com      |            |          |           | google_site_web |
        | www.google.com       |            |          |           | google_site_web |
        +----------------------------------------------------------------------------+

This displays all the information known about the one host we captured in 
[Example 1: Finding Hosts With google_site_web]

You can also query the database from within Recon-ng via the query command.

`[recon-ng][default] >` **`query`**

        Usage: query <sql>
            SQL Examples:
                SELECT columns|* FROM table_name
                SELECT columns|* FROM table_name WHERE some_column=some_value
                DELETE FROM table_name WHERE some_column=some_value
                INSERT INTO table_name (column1, column2,...) VALUES (value1, value2,...)
                UPDATE table_name SET column1=value1, column2=value2,... WHERE some_column=some_value

This gives a basic overview of information retrieval in Recon-ng.

Next we will look at your options for reporting.

Example 3: Reporting From Within Recon-ng
-----------------------------------------

From within Recon-ng, and assuming you already have data stored to report on, run:

`[recon-ng][default] >` **`use reporting`**

        [*] Multiple modules match 'reporting'.
        
          Reporting
          ---------
            reporting/csv
            reporting/html
            reporting/list
            reporting/pushpin
            reporting/xml

This will give you an overview of the available reporting modules.  

My favorite report format is html.

`[recon-ng][default] >` **`use reporting/html`**

Next, show the options available to the module.

`[recon-ng][default][html] >` **`show options`**

        Name      Current Value                                         Req  Description
        --------  -------------                                         ---  -----------
        CREATOR   ADHD                                                  yes  creator name for the report footer
        CUSTOMER  ADHD                                                  yes  customer name for the report header
        FILENAME  /home/adhd/.recon-ng/workspaces/default/results.html  yes  path and filename for report output
        SANITIZE  True                                                  yes  mask sensitive data in the report

I have previously set anything I would need.

If you need help setting the options, refer back to the Recon-ng Usage above.

Once you're ready to go, simply:

`[recon-ng][default][html] >` **`run`**

        [*] Report generated at '/home/adhd/.recon-ng/workspaces/default/results.html'.

The report has been created.  Let's view it.

From another terminal run:

`~$` **`firefox /home/adhd/.recon-ng/workspaces/default/results.html &`**

There you go!!

