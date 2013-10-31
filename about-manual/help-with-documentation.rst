.. include:: /links.txt

Hilf mit bei der Dokumentation
==============================

Eine Dokumentation zu schreiben ist jede Menge Arbeit. Hilfe dabei ist daher
gerne gesehen.

Allgemeine Regeln zur Dokumentation
-----------------------------------

* Dieses Handbuch wird unter der Creative Commons `by-nc-sa`_ Lizenz veröffentlicht.
  Deine Beiträge werden ebenfalls unter dieser Lizenz freigegeben.
* Texte sinnvoll durch Überschriften gliedern
* Bilder: Nur JPG oder PNG verwenden
* Wo möglich verweise auf andere Stellen in der Dokumentation
* Erkläre möglichst gut und dabei so kurz wie möglich
* komplette Screenshots sollten 1024px breit sein

Quellcode dieser Dokumentation
------------------------------

Der Quellcode diese Dokumentation befindet sich auf Github:
`Meshkit Dokumentation <http://www.github.com/freifunk/meshkit-doc>`_.


..  Benutzung von Sphinx
..  --------------------

Zur Erstellung der Dokumentation wird `Sphinx`_ genutzt.

Installation von Sphinx
=======================

Viele Linux Distributionen bieten Sphinx als Paket an, unter Debian kann es
installiert werden mit:

.. code-block:: bash

  aptitude install python-sphinx python-pygments

Desweiteren benötigt diese Dokumentation ein angepasstes Theme, das mit
folgendem Befehl installiert werden kann:

.. code-block:: bash

  easy_install sphinxjp.themes.basicstrap

oder alternativ mit

.. code-block:: bash

  pip install sphinxjp.themes.basicstrap


Formatierung mit rst
====================

Überschriften
-------------

Überschriften sollen Texte logisch Gliedern. Mehr als 4 Ebenen sind
in der Regel nicht sinnvoll.

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - Level
     - Unterstreichen mit
   * - 1 (Kapitelüberschriften)
     - \=
   * - 2
     - \=
   * - 3
     - \-
   * - 4
     - \^


Codebeispiele
-------------

Codeblöcke immer kennzeichnen mit

.. code-block:: rst

  .. code-block:: sh
     while true; do
       ping -n 3 google.de
     done

was dann so aussieht:

.. code-block:: sh

   while true; do
     ping -n 3 google.de
   done

Menüpfade zu Seiten in der GUI
------------------------------

Es gibt speziell für GUI-Pfade ein Label. Dieses sollte für alle
Menüpfade verwendet werden, z.B.

.. code-block:: rst

   :menuselection:`Administration --> System --> Administration`

Das sieht dann so aus:

:menuselection:`Administration --> System --> Administration`

Verweise auf andere Elemente in der GUI
---------------------------------------

Auch hierfür gibt es ein Label:

.. code-block:: rst

   :guilabel:`Irgendwas in der GUI`

was dann so aussieht:

:guilabel:`Irgendwas in der GUI`

Dateipfade
----------

Pfade zu Dateien und Verzeichnissen werden durch das Label \:file:
kenntlich gemacht, z.B.

.. code-block:: rst

   :file:`/etc/passwd`

was dann so aussieht:

:file:`/etc/passwd`

Kommandos
---------

Kommandos auf der Shell sollten durch das Label \:command:
kenntlich gemacht werden, z.B.

.. code-block:: rst

   :command:`/etc/init.d/olsrd restart`

was dann so aussieht:

:command:`/etc/init.d/olsrd restart`

Glossar
-------

Abkürzungen wie z.B. DHCP sollten im Glossar (:file:`glossar.rst`)
erklärt werden. Um Begriff dann auf die entsprechende Stelle im Glossar
zu verlinken wird **:term:** verwendet, z.B.

.. code-block:: rst

   Foo kann mit :term:`DHCP` konfiguriert werden

Das sieht im Text dann so aus:

Foo kann mit :term:`DHCP` konfiguriert werden


Weiterführende Links zu Sphinx und rst
======================================

* `Sphinx Dokumentation <http://sphinx-doc.org/contents.html>`_
* `Sphinx Doku bei grund-wissen.de <http://www.grund-wissen.de/linux-und-opensource/tools/sphinx/index.html>`_
