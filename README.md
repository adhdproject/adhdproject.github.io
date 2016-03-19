Generating HTML from Markdown Using Pandoc
==========================================

#  Notes
* The following command works on OSX and makes it easy to specify all the tool docs. However, Pandoc on OSX seems to be doing some funky stuff when parsing blocks of text and turning them into paragraph tags.
* `(cd Tools && pandoc -r markdown_mmd -w html -o ../Usage_Docs_no_cover.html --toc --self-contained --css=format.css ../ADHD/Credentials.md *.md)`

* On Windows you cannot specify a glob for some reason so you need to list out each individual file like so. However, Pandoc on Windows makes prettier HTML.
* `pandoc -r markdown_mmd -w html -o ../Usage_Docs_no_cover.html --toc --self-contained --css=format.css ../ADHD/Credentials.md Artillery.md BearTrap.md Beef.md Cryptolocked.md Decloak.md DenyHosts.md Ghostwriting.md HoneyBadger.md HoneyPorts.md Jar-Combiner.md Kippo.md Nova.md OsFuscate.md Pushpin.md Recon-ng.md SET.md Spidertrap.md WebBugServer.md Weblabyrinth.md`

* Ugh, now there is a difference the other way with this command. On Windows, the tables get messed up but on OSX, thery are fine.
* `pandoc -r html -w html -o Usage_Docs.html --self-contained --css=Tools/format.css Cover.html Usage_Docs_no_cover.html`

* I was using Pandoc v.1.12.4.2-1 on Windows and v.1.12.3 on OSX (Homebrew) so the differences could very well be in the version used. Latest version out as of now is 1.13.0.1 so these issues could be fixed.

* Each tool file must start/end with a couple newlines.  This is because when combined, Pandoc just concatenates them together and with the newline buffers between each doc, the markdown gets run together and the formatting is messed up.

# Copy-Paste for Windows
```
pandoc -r markdown_mmd -w html -o Cover.html --self-contained ADHD/CoverSheet.md
cd Tools
pandoc -r markdown_mmd -w html -o ../Usage_Docs_no_cover.html --toc --self-contained --css=format.css ../ADHD/Credentials.md Artillery.md BearTrap.md Beef.md Cryptolocked.md Decloak.md DenyHosts.md Ghostwriting.md HoneyBadger.md HoneyPorts.md Jar-Combiner.md Kippo.md Nova.md OsFuscate.md Pushpin.md Recon-ng.md SET.md Spidertrap.md WebBugServer.md Weblabyrinth.md
cd ..

::pandoc -r html -w html -o Usage_Docs.html --self-contained --css=Tools/format.css Cover.html Usage_Docs_no_cover.html
```

# Copy-Paste for OSX
```
#pandoc -r markdown_mmd -w html -o Cover.html --self-contained ADHD/CoverSheet.md
#(cd Tools && pandoc -r markdown_mmd -w html -o ../Usage_Docs_no_cover.html --toc --self-contained --css=format.css ../ADHD/Credentials.md *.md)

pandoc -r html -w html -o Usage_Docs.html --self-contained --css=Tools/format.css Cover.html Usage_Docs_no_cover.html
```