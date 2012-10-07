Bancha Documentation
=====================

This documentation uses Sphinx for generating the actual HTML.

Requirements
------------

You can read all of the documentation within as its just in plain text files, marked up with ReST text formatting.  To build the documentation you'll need the following:

* Make
* Python
* Sphinx

You can install sphinx using:

	easy_install sphinx

Building the documentation
--------------------------

After installing the require packages you can build the documentation using `Make`

	# Create all the HTML docs.
	make html

This will generate all the documentation in an html form.  Other output such as 'pdf' and 'htmlhelp' are not fully complete at this time.

After making changes to the documentation, you can build the html version of the docs by using `make clean; make html` again.  This will build only the html files that have had changes made to them.

