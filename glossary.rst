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

