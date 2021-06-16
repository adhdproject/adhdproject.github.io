WWHF 2021 Web-Labyrinth
----------------------
Before you do this lab, I would recommend taking a look at the Web Labyrinth tool usage documentation located at adhdproject.github.io This lab will demonstrate how Web Labyrinth could be implemented on a real website.

Check out the Site
------------------
On your local ADHD machine, browse to http://127.0.0.1/waywest-labyrinth/index.php
You should see a totally normal page with nothing interesting whatsoever! Let's verify that. Use wget to crawl the webpage. 
`~$`  **`wget -m http://127.0.0.1/waywest-labyrinth/index.php/`**
You should see weblabyrinth at work trapping the spider! This works curtesy of a simple html trick. We can link to our weblabyrinth server with a link embedded into a 1x1 pixel image. This looks totally normal to an average user, but anyone trying to crawl our site will be in for a surprise!

---------------
You should be good to continue with your vm after this lab, but if you like to be safe go ahead and reboot. If you're participating in the CTF at Way West, you might want to take a closer look at the output in the terminal after running wget!
