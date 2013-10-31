Pakete installieren
===================

Auf OpenWrt-Systemen können weitere Pakete nachinstalliert werden,
um den Funktionsumfang des Routers zu vergrössern und um bestimmte
Aufgaben zu erfüllen.

Die Pakete werden durch ``Repositories`` bereitgestellt, die in
:file:`/etc/opkg.conf` festgelegt sind. Jedes Repository enthält eine
Datei ``Packages``, die eine Liste aller dort verfügbaren Pakete und
Informationen zu diesen (Grösse, Beschreibung, Abhängigkeiten...)
enthält. Diese Listen können relativ gross sein und werden deshalb
nicht permanent auf dem Router gespeichert. **Deshalb müssen, bevor Pakete
installiert werden können, die Paketkisten aktualisiert werden**.

.. hint::

   Wenn man schon im voraus weiss, welche Pakete man zusätzlich zu den
   Standardpaketen auf dem Router benötigt dann ist es sinnvoll, diese
   gleich von Meshkit mit ins Firmwareimage bauen zu lassen, da dadurch
   durch bessere Kompression Platz im Flash-Speicher des Routers gespart
   werden kann. Siehe: :ref:`meshkit-packages`.

.. hint::

   Der Router braucht eine Internetverbindung damit Pakete installiert
   werden können.


Im Webinterface
---------------

Pakete lassen sich in LuCI unter
:menuselection:`Administration --> System --> Paketverwaltung` verwalten.

.. image:: /images/luci/paketverwaltung.jpg

Im Tabmenü bei (1) kann :guilabel:`Aktionen` oder :guilabel:`Konfiguration`
gewählt werden. Unter Konfiguration kann die opkg-Konfiguration angepasst werden.
Darauf gehen wir hier jedoch nicht weiter ein.

(3) zeigt den noch verfpgbaren Platz im Flash Speicher des Routers an,
der noch für die Installation von Paketen zur Verfügung steht.

Pakete installieren
^^^^^^^^^^^^^^^^^^^

Zunächst müssen, sofern noch nicht geschehen, die Paketlisten durch einen
Klick auf :guilabel:`Listen aktualisieren` (2) heruntergeladen werden. Danach
können Pakete auf verschiedene Art installiert werden:

* Durch direkte Angabe eines Paketnamens unter :guilabel:`Paket herunterladen
  und installieren` (4).
* Indem man nach einem Paket unter :guilabel:`Filter` sucht (5).
* Durch Auswählen aus der Liste unten im Tab :guilabel:`Verfügbare Pakete` (7)

Pakete deinstallieren
^^^^^^^^^^^^^^^^^^^^^

Durch KLicken auf :guilabel:`Entfernen` im Tab :guilabel:`Installierte Pakete` (6)
können einzelne Pakete entfernt werden.


Mit SSH
-------

Das Kommando zum Arbeiten mit Paketen heisst :command:`opkg`. 

.. list-table::
   :widths: 40 60
   :header-rows: 1

   * - Aktion
     - Kommando
   * - Hilfe anzeigen
     - opkg -h
   * - Paketlisten updaten
     - opkg update
   * - Paket suchen
     - opkg find \*<Suchbegriff>\*
   * - Info zu einem paket anzeigen
     - opkg info <Paketname>
   * - Paket installieren
     - opkg install <Paketname>
   * - Paket entfernen
     - opkg remove <Paketname>
   * - Installierte Pakete anzeigen
     - opkg list_installed

Werden Dienste (wie z.B. ein FTP-Server) installiert, die durch ein
init-Script in :file:`/etc/init.d/` gestartet werden dann muss das
betreffende init-Script noch aktiviert und der Dienst danach gestartet
werden. Als Beispiel hier für den FTP-Server ``vsftpd``:

.. code-block:: sh

   opkg update
   opkg install vsftpd
   /etc/init.d/vsftpd enable
   /etc/init.d/vsftpd start


Offlineinstallation von Paketen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Hat der Router keine Internetverbindung dann können Pakete installiert
werden, indem sie in den :file:`/tmp`-Ordner des Routers kopiert
werden und dann mit :command:`opkg install /tmp/<paketname>` installiert
werden. Zum Kopieren von Dateien auf den Router siehe: :ref:`scp`.

Weiterführende Links
--------------------

* `opkg im OpenWrt Wiki <http://wiki.openwrt.org/doc/techref/opkg>`_
* `packages im OpenWrt Wiki <http://wiki.openwrt.org/doc/packages>`_
