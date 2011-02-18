# Cerb5 User's Guide #

Copyright (C) 2011, WebGroup Media LLC.

<http://www.cerberusweb.com/>

Introduction
------------

These are the raw materials required to build the _Cerb5 User's Guide_ in a variety of formats (HTML, ePub, PDF, Docbook, etc).

Requirements
------------

The book is in Markdown format.  To modify and extend the documentation you only need a text editor.

As defined by Wikipedia:

> Markdown is a lightweight markup language, originally created by John Gruber and Aaron Swartz to help maximum readability and "publishability" of both its input and output forms. The language takes many cues from existing conventions for marking up plain text in email.

You need to install [Pandoc](http://johnmacfarlane.net/pandoc/) to convert the source files into production documentation.

You can reference Pandoc's Markdown syntax here:
<http://johnmacfarlane.net/pandoc/README.html#pandocs-markdown>


Creating an HTML manual
-----------------------

	$ pandoc -s -c _html/html.css -s -S --toc -N -o index.html en/*.markdown


Credits
-------

This project makes use of the following technology:

* [Pandoc](http://johnmacfarlane.net/pandoc/) by John MacFarlane
* [Markdown](http://daringfireball.net/projects/markdown/) by John Gruber
* [OpenOffice](http://www.openoffice.org/) by Oracle and its open source contibutors
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

