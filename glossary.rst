.. include:: /links.txt

Glossar
*******

.. glossary:: :sorted:

   Alias-Interface
      Schnittstellen können mehrere IP-Adressen haben. Die erste IP der
      Schittstelle ist die primäre IP. Weitere IP-Adressen auf der
      Schittstelle sind Alias-IPs. Da bei OpenWrt Aliase als eigene
      Schnittstellen angelegt werden, sprechen wir hier auch von
      Alias-Interface.


   DHCP
      Dynamic Host Configuration Protokoll. Dies wird benutzt, um Rechnern
      im Netzwerk automatisch IPv4-Adressen und weitere Einstellungen wie
      Defaultgateway oder :term:`DNS`-Server zuzuweisen.

   DNS
      Domain Name System

   HNA
      Host/Network Announcement. Damit werden von :term:`OLSR` stellvertretend
      einzelne Rechner oder Netzwerke im Mesh angekündigt.

   JSON
     JavaScript Object Notation ist ein Datenformat, das für Mensch und Computer
     einfach lesbar sein soll. Es wird vor allem zum Austausch von Daten zwischen
     Programmen benutzt.
     Siehe: http://de.wikipedia.org/wiki/JavaScript_Object_Notation

   MAC
     Weltweit eindeutige Hardwareadresse von Netzwerkschnittstellen. Hat die
     Form XX:XX:XX:XX:XX:XX.

   MID
     Hat ein Knoten mehrere Interfaces auf denen :term:`OLSR` läuft dann wird eine
     davon zur primären Schnittstelle erklärt und deren IP als OLSR-Haupt-IP-Adresse
     verwendet. Die IP-Adressen weiterer Schnittstellen werden als MID bezeichnet.

   Netzmaske
      Die Netzmaske oder auch Subnetzmaske gibt die Grösse von Netzwerken an.
      Die Netzmaske kann entweder in CIDR-Notation, z.B. /24 oder in der
      dotted decimal notation, z.B. 255.255.255.0 beschrieben werden.
      Rechner die in einem von der Netzmaske abgedeckten IP-Bereich liegen
      werden als direkt erreichbar angesehen. Ein nützliches Tool zum Berechnen
      von Netzmasken ist :command:`ipcalc`. Mehr zu Netzmasken bei Wikipedia:
      http://de.wikipedia.org/wiki/Netzmaske

   OLSR
      Optimized Link State Routing. OLSR ist ein Routingprotokoll für
      Meshnetzwerke, das von der Meshkit Firmware benutzt wird.
      `OLSRd`_-Homepage.

   TFTP
      Trivial File Transfer Protocol. Dies wird häufig genutzt, um
      Firmwareimages zum Router zu übertragen.

   UCI
      Unified Configuration Interface. Vereinheitlicht die Konfiguration
      des OpenWrt Systems. Zur Benutzung von des :command:`uci`-Kommandos
      auf der Kommandzeile siehe :ref:`shell-uci`.

   VAP
      Virtueller Access Point. Sofern die Hardware dies unterstützt, können
      mehrere voneinander getrennte WLAN-Netzwerke auf einer einzigen
      WLAN-Netzwerkkarte betrieben werden. Wird unterstützt von Hardware die
      einen der folgenden Treiber verwendet: ``ath5k``, ``ath9k``, ``madwifi``.
      Welche Treiber ein Router benutzt findet man in den meisten Fällen über die
      OpenWrt `Table of Hardware`_ heraus.
