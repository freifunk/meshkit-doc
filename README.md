Meshkit Dokumentation
=====================

This is intended to be a user oriented dokumentation for the [Meshkit Firmware](http://meshkit.freifunk.net "Meshkit Freifunk Firmware Generator).
It is in german language only at this point.

The documentation is not complete yet (not even nearly) and WIP. If you can help
with making the Meshkit Documentation better then please do so. Thanks in advance.


It uses [Sphinx](http://sphinx-doc.org/ "Sphinx Documentation") to create the pages.

Contribute
----------

To contribute you must first install some depencies, on Debian this can be done
with the following commands:

    aptitude install python-sphinx python-pygments
    easy_install sphinxjp.themes.basicstrap

After that clone this repository and change into the meshkit-doc directory. All source files
can be edited in the source/ folder. After you are done editing:

    make html

to build the html pages. You will find them in build/html/.

If your changes look good push them to this repository or open a pull request.
