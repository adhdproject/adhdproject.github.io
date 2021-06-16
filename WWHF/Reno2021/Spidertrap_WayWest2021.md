Spidertrap
=====

Introduction
---------
Spidertrap is a tool designed to annoy and confuse attackers that are utilizing web spiders (hence the name). Spidertrap accomplishes this by displaying a list of hyperlinks that are dynamically generated. It does this using a random link generator or using a file of links.

For the purposes of these labs, we will be running Spidertrap using a pre-prepared list of links. To start Spidertrap, run the following commands:
```
cd /opt/spidertrap
python3 spidertrap.py waywest.txt
```

Then, browse to http://localhost:8000 via Firefox. You should be presented with a long list of links.

Lab 1
-----
Check out the list of links that are displayed. There are many.
Take a scroll and see if anything sticks out to you.
You may need to click a few times before you see it, and it's hiding rather well.

Lab 2
-----
You may have seen some other link names that strike you as interesting.
The values are surrounded by `st2.x{...}` instead of `flag{...}`, which indicates there's more that needs to be done. There are 14 components to this flag (0 - 13). You may find the following Wikipedia article useful:
https://en.wikipedia.org/wiki/List_of_file_signatures
Consider writing a simple web crawler that can automatically put all the pieces together.

Lab 3
-----
Time to get a bit mean. Like the second challenge, there are a number of entries
surrounded by `st3.x{...}`. This time, there are 29 components to the flag (0 - 28).
What might the numbers mean? Looking at things differently will help. When you're on
the right track, you may see it if you squint a little. Some Python may be recommended to
help put the pieces together...
