.. include:: /links.txt

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


to be continued (TODO)


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
