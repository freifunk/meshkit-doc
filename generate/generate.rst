.. include:: /links.txt

.. _meshkit-fulldoc:

Firmware mit Meshkit generieren lassen
======================================

Das Erstellen von Firmwareimages erfolgt in drei Schritten:

1. Grundlegende Systemeinstellungen
-----------------------------------

Öffne die Meshkit_ Webseite im Browser. Du siehst nun folgendes:

.. image:: /images/meshkit/basic-settings.jpg


Erklärung der einzelnen Optionen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``1. Login bzw. Registrieren``

Hier kann man einen User für Meshkit registrieren. Dadurch wird es möglich,
einige Datenfelder beim Generieren neuer Images bereits auszufüllen, z.B.
Community, Adresse oder SSH Public Keys. Es ist nicht notwendig einen Benutzer
im Meshkit zu registrieren. Wer aber öfter Images generiert dem kann
die Registrierung ersparen, bei einigen Feldern immer und immer wieder die
selben Daten einzugeben.

``2. Geräte Vorauswahl``

Im unteren Bereich von Meshkit befinden sich einige Links zu Geräten, die
häufig verwendet werden. Klickt man auf einen dieser links, dann wählt Meshkit
automatisch das richtige Target (und in Schritt 2 auch das passende Profil) für
dieses Gerät. Wenn ein Gerät nicht in dieser Liste steht dann heisst das nicht,
dass es nicht unterstützt wird, sondern nur, dass man Target und im nächsten
Schritt Profil manuell auswählen muss.

``3. Einstellungen``

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - Option
     - Beschreibung
   * - Community
     - Wähle deine Community aus. Wenn es noch keine Community für deine Stadt
       gibt dann siehe :ref:`community_profile_create`.
   * - Target
     - Wähle ein passendes Target (siehe :ref:`Infos zum Router sammeln`)
   * - Expertenmodus
     - Wenn ausgewählt, dann werden im nächsten Schritt wesentlich mehr Optionen
       zur Konfiguration des Routers angezeigt.
   * - Keine Konfiguration
     - Erstellt ein Image, es wird aber keine Konfiguration durchgeführt. Dies
       ist vor allem nützlich um Images zu erhalten, die für sysupgrade (TODO:
       Seite zu sysupgrade anlegen) verwendet werden können.
   * - Email
     - Wenn angegeben wird nachdem das Image gebaut wurde eine Mail an diese
       Adresse geschickt.


``4. Absenden``

Nachdem alle Einstellungen getätigt wurde klicke auf **Absenden** um zu
Schritt 2 des Meshkits zu gelangen, wo weitere Einstellungen vorgenommen
werden müssen.


2a Konfiguration der Firmware (Normalmodus)
-------------------------------------------

In diesem Schritt geht es an die eigentliche Konfiguration der Firmware
im ``Normalmodus``. Wenn du im vorherigen Kapitel die Option
``Èxperteneinstellungen`` gewählt hast dann gehe gleich weiter zum
nächsten Kapitel.

Auf der Seite sind nun einige Tabs zu sehen. Diese kann man nacheinander
durchgehen und alle Einstellungen vornehmen.

.. hint::
  Viele Felder sind schon mit sinnvollen Optionen vorbelegt. Ist man als
  User im Meshkit eingeloggt werden auch Kontaktdaten etc. vorausgefüllt.

Systemeinstellungen
^^^^^^^^^^^^^^^^^^^

Hier können allgemeine Systemeinstellungen vorgenommen werden.

.. image:: /images/meshkit/settings-normal-system.png

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Option
     - Beschreibung
     - Default
   * - Profil
     - Wählt ein Hardwareprofil für den Router aus. Wenn Du im vorigen
       Schritt ein Routermodell aus der Liste unten ausgewählt hast, dann ist
       das Profil bereits richtig ausgewählt. Ansonsten wähle hier das zu
       deinem Router passende Profil. Siehe dazu die OpenWrt
       `Table of Hardware`_
     - generisches Profil
   * - Name des Knotens
     - Ein im Netzwerk eindeutiger Name für den Knoten.
     - IP-Adresse des ersten Wifi Interfaces. Punkte werden dabei durch
       Striche ersetzt, z.B. 10-0-0-1


Standort
^^^^^^^^

Hier sollten ein paar Daten zum Standort des Routers angegeben werden.
Länge und Breite sind notwendig, um den Router automatisch auf Karten
des Netzwerks platzieren zu können.

.. image:: /images/meshkit/settings-normal-location.png

Klickt man auf den ``Karte anzeigen`` Button öffnet sich eine Karte,
in der man durch klicken auf einen Punkt die Werte ``geogr. Länge`` und
``geogr. Breite`` automatisch setzen kann.

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Option
     - Beschreibung
     - Default
   * - Standort
     - Standort des Routers als Text, z.B. Strasse und Ort
     - (keiner)
   * - geogr. Länge
     - Längenangabe zur Position des Routers
     - definiert durch Community-Profil, siehe :ref:`community_profiles`
   * - geogr. Breite
     - Breitenangabe zur Position des Routers
     - definiert durch Community-Profil, siehe :ref:`community_profiles`

Kontakt
^^^^^^^

Hier werden Kontakteinstellungen usw. vorgenommen, die auch auf
der öffentlichen Webseite des Freifunk Routers angezeigt werden.
Es ist empfehlenswert zumindest einen Nickname und eine gültige
Emailadresse anzugeben, damit interessierte Nutzer oder andere
Betreiber von Freifunkknoten Kontakt aufnehmen können.

.. image:: /images/meshkit/settings-normal-contact.png

Wireless
^^^^^^^^
Einstellungen für ein oder mehrere WLAN-Schnittstellen. Für Geräte mit
mehreren WLAN-Interfaces können durch klicken
auf ``Ein weiteres Wirelessdevice hinzufügen`` insgesamt bis zu
drei WLAN-Schnittstellen konfiguriert werden.

.. image:: /images/meshkit/settings-normal-wireless.png

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Option
     - Beschreibung
     - Default
   * - IP-Adresse
     - eine netzweit eindeutige IP-Adresse die in den meisten Communities
       zentraler Stelle vergeben wird. Siehe: :ref:`IP-registrieren`.
       **Die IP sollte auf jeden Fall geändert werden.**
     - Erste IP im Netzwerk der Community.
   * - Kanal
     - Der Funkkanal. Hier steht per Default der richtige Kanal
       für die gewählte Community. Ändere den Kanal daher nur wenn du
       genau weisst was du tust.
     - Voreinstellung aus Community Profil, siehe :ref:`community_profiles`

Abschicken
^^^^^^^^^^

Nachdem alle Einstellungen getätigt sind drücke im Tab ``Abschicken``
noch auf ``Submit``. Sind alle Eingaben gültig kommst du zum letzten Schritt
des Meshkit, in dem deine Firmware gebaut wird. Bei ungültigen Eingaben weisst
Meshkit darauf hin, welche Felder ungültig sind.

.. image:: /images/meshkit/settings-submit.png

-> Weiter zu :ref:`meshkit-fulldoc-generate`


2b Konfiguration der Firmware (Expertenmodus)
---------------------------------------------

In diesem Schritt geht es an die eigentliche Konfiguration der Firmware
im ``Expertenmodus``. Es ist möglich, hier mehr Einstellungen selbst
anzupassen.

Auf der Seite sind nun einige Tabs zu sehen. Diese kann man nacheinander
durchgehen und alle Einstellungen vornehmen.

.. hint::
  Viele Felder sind schon mit sinnvollen Optionen vorbelegt. Ist man als
  User im Meshkit eingeloggt werden auch Kontaktdaten etc. vorausgefüllt.


Systemeinstellungen
^^^^^^^^^^^^^^^^^^^

Hier können allgemeine Systemeinstellungen vorgenommen werden.

.. image:: /images/meshkit/settings-expert-system.png

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Option
     - Beschreibung
     - Default
   * - Profil
     - Wählt ein Hardwareprofil für den Router aus. Wenn Du im vorigen
       Schritt ein Routermodell aus der Liste unten ausgewählt hast, dann ist
       das Profil bereits richtig ausgewählt. Ansonsten wähle hier das zu
       deinem Router passende Profil. Siehe dazu die OpenWrt
       `Table of Hardware`_
     - generisches Profil
   * - Weboberfläche
     - Die zu installierende Weboberfläche. Wird 'none' gewählt dann wird keine
       Weboberfläche gewählt
     - LuCI
   * - Theme
     - Zu installierendes Theme. Nur Freifunk-Generic hat einige Anpassungen damit
       die Oberfläche für verschiedene Communities angepasst werden kann und ist
       daher das empfohlene Theme.
     - freifunk-generic
   * - Name des Knotens
     - Ein im Netzwerk eindeutiger Name für den Knoten.
     - IP-Adresse des ersten Wifi Interfaces. Punkte werden dabei durch
       Striche ersetzt, z.B. 10-0-0-1
   * - IPv6
     - IPv6 aktivieren. Wird nur angezeig wenn IPv6 im Communityprofil
       aktiviert ist.
     - durch Communityprofil vorgegeben, siehe :ref:`community_profiles`
   * - SSH Public Keys
     - SSH Public Keys, einer pro Zeile. Rechner, deren öffentlicher SSH-Schlüssel
       hier gepeichert ist können sich mit diesem Key beim SSH-Server
       authentifizieren
     - keine

Standort
^^^^^^^^

Hier sollten ein paar Daten zum Standort des Routers angegeben werden.
Länge und Breite sind notwendig, um den Router automatisch auf Karten
des Netzwerks platzieren zu können.

.. image:: /images/meshkit/settings-normal-location.png

Klickt man auf den ``Karte anzeigen`` Button öffnet sich eine Karte,
in der man durch klicken auf einen Punkt die Werte ``geogr. Länge`` und
``geogr. Breite`` automatisch setzen kann.

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Option
     - Beschreibung
     - Default
   * - Standort
     - Standort des Routers als Text, z.B. Strasse und Ort
     - (keiner)
   * - geogr. Länge
     - Längenangabe zur Position des Routers
     - definiert durch Community-Profil, siehe :ref:`community_profiles`
   * - geogr. Breite
     - Breitenangabe zur Position des Routers
     - definiert durch Community-Profil, siehe :ref:`community_profiles`

Kontakt
^^^^^^^

Hier werden Kontakteinstellungen usw. vorgenommen, die auch auf
der öffentlichen Webseite des Freifunk Routers angezeigt werden.
Es ist empfehlenswert zumindest einen Nickname und eine gültige
Emailadresse anzugeben, damit interessierte Nutzer oder andere
Betreiber von Freifunkknoten Kontakt aufnehmen können.

.. image:: /images/meshkit/settings-normal-contact.png


Wireless
^^^^^^^^

Einstellungen für ein oder mehrere WLAN-Schnittstellen. Für Geräte mit
mehreren WLAN-Interfaces können durch klicken
auf ``Ein weiteres Wirelessdevice hinzufügen`` insgesamt bis zu
drei WLAN-Schnittstellen konfiguriert werden.

.. image:: /images/meshkit/settings-expert-wireless.png

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Option
     - Beschreibung
     - Default
   * - IP-Adresse
     - eine netzweit eindeutige IP-Adresse die in den meisten Communities
       zentraler Stelle vergeben wird. Siehe: :ref:`IP-registrieren`.
       **Die IP sollte auf jeden Fall geändert werden.**
     - Erste IP im Netzwerk der Community.
   * - Kanal
     - Der Funkkanal. Hier steht per Default der richtige Kanal
       für die gewählte Community. Ändere den Kanal daher nur wenn du
       genau weisst was du tust.
     - Voreinstellung aus Community Profil, siehe :ref:`community_profiles`
   * - DHCP aktivieren
     - Per DHCP kann Gästen im WLAN automatisch eine IP Adresse zugewiesen werden.
     - ja
   * - DHCP-Bereich
     - Der IP-Bereich aus dem Adressen für DHCP vergeben werden. Liegt dieser Bereich
       innerhalb des im Communityprofil angegebenen Meshnetzwerks
       (siehe :ref:`community_profiles`), dann wird er als
       HNA von olsrd angekündigt. Liegt er ausserhalb, dann wird NAT aktiviert.
       Wird dieses Feld leer gelassen wird automatisch ein IP-Bereich aus 6.0.0.0/8 generiert.
     - Netzwerk aus 6.0.0.0/8. Dieses Netzwerk (/24) wird automatisch aus der
       IPv4-Adresse dieses Interfaces generiert.
   * - Virtueller Access Point
     - Zusätzlich zum Mesh-Interface im adhoc-Modus wird ein weiteres WLAN-Interface
       im Access Point-Modus erstellt. Dies ist nützlich, damit sich z.B. auch Clients,
       die kein adhoc beherrschen (z.B. Android-Geräte) mit dem Netzwerk
       verbinden können.
       **Dies funktioniert nur mit Treibern die das unterstützen. Im Moment sind das madwifi, ath5k und ath9k.**
     - Voreinstellung aus Community Profil, siehe :ref:`community_profiles`
   * - Router Advertisements
     - Wenn aktiviert dann werden auf dem Virtuellen Access Point Router
       Advertisements zur automatischen Konfiguration von IPv6 beim
       Client verschickt.
     - Voreinstellung aus Community Profil, siehe :ref:`community_profiles`


LAN
^^^

Hier kann die LAN-Schnittstelle des Routers konfiguriert werden.

.. hint::
  Wenn der Router mit seiner WAN-Schnittstelle an einem Netzwerk angeschlossen
  wird das den IP-Bereich 192.168.1.0/24 benutzt, dann **muss** hier eine IP
  aus einem anderen Subnetz (z.B. 192.168.2.0/24) vergeben werden.

.. image:: /images/meshkit/settings-expert-lan.png

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Option
     - Beschreibung
     - Default
   * - LAN-Protokoll
     - Hier kann gewählt werden, ob die LAN Schnittstelle als
       Schnittstelle für OLSR konfiguriert werden soll. Wird ``olsr``
       als Protokoll gewählt dann können an diese Schnittstelle
       weitere Freifunk Router per Kabel angeschlossen werden.
     - static
   * - IPv4-Adresse
     - Die IPv4-Adresse der LAN-Schnitstelle. Wurde als Protokoll ``olsr``
       gewählt dann sollte hier eine IP aus dem Mesh-Netzwerk der Community
       gewählt werden. Siehe: :ref:`IP-registrieren`
     - 192.168.1.1
   * - Netzmaske
     - Netzmaske für das LAN-Interface. Soll dieses Netzwerk ein OLSR-Netzwerk
       sein, dann hat es sich als praktisch erwiesen, hier kleinere Netzmasken
       für die lokale Vernetzung von Nodes zu verwenden.
     - 255.255.255.0
   * - DHCP aktivieren
     - Nur verfügbar wenn ``olsr`` als Protokoll gewählt wurde. Per DHCP kann Gästen
       im Netzwerk automatisch eine IP Adresse zugewiesen werden.
     - nein
   * - DHCP-Bereich
     - Der IP-Bereich aus dem Adressen für DHCP vergeben werden. Liegt dieser Bereich
       innerhalb des im Communityprofil angegebenen Meshnetzwerks
       (siehe :ref:`community_profiles`), dann wird er als
       HNA von olsrd angekündigt. Liegt er ausserhalb, dann wird NAT aktiviert.
       Wird dieses Feld leer gelassen wird automatisch ein IP-Bereich aus 6.0.0.0/8 generiert.
     - Netzwerk aus 6.0.0.0/8. Dieses Netzwerk (/24) wird automatisch aus der
       IPv4-Adresse dieses Interfaces generiert.

WAN
^^^

Hier kann die LAN-Schnittstelle des Routers konfiguriert werden.

.. hint::
  Wenn der Router mit seiner WAN-Schnittstelle an das eigene Netzwerk angeschlossen
  werden soll (siehe: :ref:`net-setup-standard`), dann solltest du hier
  den Zugriff auf die Weboberfläche und SSH erlauben, damit du aus deinem
  Netzwerk auf den Router zugreifen kannst.

.. image:: /images/meshkit/settings-expert-wan.png

Es stehen insgesamt drei Protokolle zur Auswahl:

* dhcp - Automatische Konfiguration der WAN-Schnittstelle durch DHCP
* static - Statische konfiguration
* olsr - Als OLSR-Schnittstelle konfigurieren (z.B. für Kabelkopplung von Nodes)

**Optionen für Protokoll dhcp**

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Option
     - Beschreibung
     - Default
   * - Erlaube SSH
     - Zugriff auf SSH (Port 22) in der Firewall erlauben
     - nein
   * - Erlaube Web
     - Zugriff auf die Weboberfläche (Ports 80 und 443) in der Firewall erlauben
     - nein
   * - Heimnetzwerk schützen
     - Verhindert Zugriffe aus dem Mesh auf das lokale Netzwerk hinter der WAN-Schnittstelle
     - ja
   * - Internet teilen
     - Anderen im Mesh erlauben die eigene Internetverbindung mitzunutzen. Ist diese Option aktiviert
       dann wird das olsrd dyngw_plain Plugin aktiviert. Dieses überwacht die Internetverbindung und
       kündigt einen verfügbaren Internetzugang als HNA an sobald er erkannt wurde. **Wenn die eigene
       Internetverbindung nur über einen Tunnel freigegeben werden soll, dann diese Option nicht wählen.**
     - nein

**Optionen für Protokoll static**

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Option
     - Beschreibung
     - Default
   * - IPv4 Adresse
     - IPv4 Adresse der WAN-Schnittstelle
     - keine
   * - Netzmaske
     - Netzmaske der WAN-Schnittstelle, z.B. 255.255.255.0
     - keine
   * - Gateway
     - Gateway des Netzwerks
     - keines
   * - DNS
     - DNS Server. Mehrere Server durch Leerzeichen voneinander getrennt angeben.
     - keines
   * - Erlaube SSH
     - Zugriff auf SSH (Port 22) in der Firewall erlauben
     - nein
   * - Erlaube Web
     - Zugriff auf die Weboberfläche (Ports 80 und 443) in der Firewall erlauben
     - nein
   * - Heimnetzwerk schützen
     - Verhindert Zugriffe aus dem Mesh auf das lokale Netzwerk hinter der WAN-Schnittstelle
     - ja
   * - Internet teilen
     - Anderen im Mesh erlauben die eigene Internetverbindung mitzunutzen. Ist diese Option aktiviert
       dann wird das olsrd dyngw_plain Plugin aktiviert. Dieses überwacht die Internetverbindung und
       kündigt einen verfügbaren Internetzugang als HNA an sobald er erkannt wurde. **Wenn die eigene
       Internetverbindung nur über einen Tunnel freigegeben werden soll, dann diese Option nicht wählen.**
     - nein

**Optionen für Protokoll olsr**

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Option
     - Beschreibung
     - Default
   * - IPv4-Adresse
     - Eine IP aus dem Mesh-Netzwerk der Community. Siehe: :ref:`IP-registrieren`
     - keine
   * - Netzmaske
     - Netzmaske für das WAN-Interface. Es sich als praktisch erwiesen, für
       kabelkopplungen kleinere Netzmasken zu verwenden.
     - keine

.. _meshkit-packages:

Pakete
^^^^^^

Hier kann die Liste installierter Pakete angepasst werden und so eigene noch
benötigte Pakete direkt ins Image gebaut werden. Dies ist auch deshalb praktisch,
da direkt ins Image gebaute Pakete dank besserer Kompression weniger Platz
beanspruchen als später nachinstallierte Pakete. 

Um Pakete zu installieren den Paketnamen im Textfeld hinzufügen. Alternativ können
Pakete auch aus der Liste unten durch einen Klick auf das "+"-Symbol hinzugefügt
werden.

.. image:: /images/meshkit/settings-expert-packages.png

Upload
^^^^^^

Über dieses Formular kann ein .tar.gz Archiv mit eigenen Dateien hochgeladen
werden, die ins Firmwareimage kopiert werden sollen. Dadurch lassen sich leicht
eigene Anpassungen direkt mit ins resultierende Image packen.

.. image:: /images/meshkit/settings-expert-upload.png

**Beispiel:** hallowelt.html soll auf dem router in /www erstellt werden.
Um diese Datei zu erstellen und zu packen kann man unter Linux so
vorgehen:

.. code-block:: sh

  cd /tmp
  mkdir www
  echo "Hallo Welt" > www/hallowelt.html
  tar -cvzf upload.tar.gz www

Das Archiv ``upload.tar.gz`` kann nun über den Button ``Browse`` ausgewählt
und hochgeladen werden. Nach dem Flashen des Routers befindet sie sich auf dem
Router im /www Ordner.

Abschicken
^^^^^^^^^^

Nachdem alle Einstellungen getätigt sind drücke im Tab ``Abschicken``
noch auf ``Submit``. Sind alle Eingaben gültig kommst du zum letzten Schritt
des Meshkit, in dem deine Firmware gebaut wird. Bei ungültigen Eingaben weisst
Meshkit darauf hin, welche Felder ungültig sind.

.. image:: /images/meshkit/settings-submit.png


.. _meshkit-fulldoc-generate:

3. Firmware erstellen
---------------------

Die Firmware wird nun erstellt. Das dauert 20s bis einige Minuten. Wenn Meshkit
damit fertig ist, zeigt es Links zu den gebauten Images an:

.. image:: /images/meshkit/build-firmware-results.png

Lade nun die passende Firmwaredatei für den Freifunk Router auf
deinen Computer herunter (Rechtsklick -> Speichern unter).
oder kopiere den Link (Rechtsklick -> "Link Adresse kopieren" oder
dergleichen).

Die eben erstellte Firmware muss dann noch auf den Router geflasht werden:
:ref:`Flashen` 
