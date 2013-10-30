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
