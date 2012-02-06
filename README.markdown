# Cerb5 User's Guide #

Copyright (C) 2012, WebGroup Media LLC.

<http://www.cerberusweb.com/>

Introduction
------------

These are the raw materials required to build the _Cerb5 User's Guide_ in a variety of formats (HTML, ePub, PDF, Docbook, etc).

Requirements
------------

The book utilizes the Sphinx documentation engine, and the files are in reStructuredText format.  To modify and extend the documentation you only need a text editor.

You need to install [Sphinx](http://sphinx.pocoo.org/) to convert the source files into production documentation.

You can reference reStructuredText syntax here:
<http://sphinx.pocoo.org/rest.html>


Creating an HTML manual
-----------------------

    $ cd /path/to/cerb5-book
	$ sphinx-build -b html -d _build/doctrees . _build/html

Credits
-------

This project makes use of the following technology:

* [Python](http://python.org/) by Python Software Foundation and its contributions
* [Sphinx](http://sphinx.pocoo.org/) by The Pocoo Team
* [reStructuredText](http://docutils.sourceforge.net/rst.html) by Python Software Foundation and its contributors
* [OpenOffice](http://www.openoffice.org/) by Oracle and its contibutors
* [ImageMagick](http://www.imagemagick.org/) by ImageMagick Studio LLC
* [TextMate](http://macromates.com/) by Macromates Ltd

License
-------

The _Cerb5 User's Guide_ by WebGroup Media LLC is licensed under a [Creative Commons Attribution-ShareAlike 3.0 Unported License](http://creativecommons.org/licenses/by-sa/3.0/).

You are free:

* **to Share** -- to copy, distribute and transmit the work
* **to Remix** -- to adapt the work

Under the following conditions:

* **Attribution** -- You must attribute the work in the manner specified by the author or licensor (but not in any way that suggests that they endorse you or your use of the work).
* **Share Alike** -- If you alter, transform, or build upon this work, you may distribute the resulting work only under the same or similar license to this one.

