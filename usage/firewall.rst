Firewall
========

Standardmässig ist auf jedem Node eine Firewall aktiv. Die Regeln der
Firewall bestimmen, welche eigehenden Datenpakete das Node erreichen
bzw- weitergeleitet werden dürfen.

.. hint::

   Ausführlichere Doku zur OpenWrt Firewall und einzelnen Optionen gibt es
   im OpenWrt Wiki: http://wiki.openwrt.org/doc/uci/firewall (englisch)

.. _firewall-zones:

Firewallzonen
-------------

OpenWrt benutzt ein Konzept mit Zonen, denen eine oder mehrere Schnittstellen
zugeordnet werden. Standardmässig gibt es in der Meshkit Firmware drei Zonen:

.. list-table::
   :widths: 10 30 20 20 20
   :header-rows: 1

   * - Zone
     - Beschreibung
     - INPUT
     - FORWARD
     - NAT
   * - WAN
     - Zone für Interfaces die direkt mit dem Internet verbunden sind
     - verboten
     - verboten
     - ja
   * - LAN
     - Zone für lokale Interfaces
     - erlaubt
     - erlaubt in alle anderen Zonen
     - nein
   * - Freifunk
     - Zone für Interfaces die Teil des öffentlichen Meshs sind
     - verboten, einzelne Ports erlaubt
     - erlaubt nach WAN und Freifunk
     - teilweise

Das heisst:

* die WAN-Zone erlaubt per Default weder einkommende Verbindungen (INPUT) noch
  wird Traffic der über die WAN-Zone hereinkommt in andere Zonen weitergeleitet
  (FORWARD). Ausgehender Datenverkehr wird genatted, d.h. die Quelladresse auf
  die Adresse der ausgehenden Schnittstelle umgeschrieben (Masquerading).
* Die LAN-Zone erlaubt jeglichen einkommenden Verkehr sowie die Weiterleitung
  von Paketen in beliebige Zonen
* Die Freifunk-Zone erlaubt keinen eingehenden Verkehr, jedoch werden einige
  Ports die für den betrieb des Knotens sind einzeln erlaubt (SSH, Weboberfläche,
  :term:`DHCP`, OLSR-Pakete usw.). Traffic darf in die Zonen WAN und LAN. jedoch nicht in
  die Zone LAN weitergeleitet werden. Weitergeleiteter Verkehr wird in der Regeln
  nicht genatted. Hier gibt es jedoch eine Ausnahme: Wird für :term:`DHCP`-Clients ein
  IP-Netzwerk benutzt, das vom lokalen Node nicht als :term:`HNA` angekündigt wird, dann
  werden Pakete aus diesem :term:`DHCP`-Netzwerk genatted.

Zoneneinstellungen im Webinterface
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Die Zoneneinstellungen der Firewall findet man im LuCI-Webinterface unter
:menuselection:`Administration --> Netzwerk --> Firewall --> Allgemeine Einstellungen`

.. warning::

   Hier solltest du Einstellungen nur verändern, wenn du genau weisst was
   du tust.

.. image:: /images/luci/firewall-zones.jpg

Zoneneinstellungen auf der Shell
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Alle Firewalleinstellungen und damit auch die Einstellungen für Zonen
sind in :file:`/etc/config/firewall` gespeichert. Diese Datei kann direkt
bearbeitet werden. Folgender Ausschnitt zeigt die Einstellungen
für die **WAN-Zone**:

.. code-block:: sh

   config zone
           option name 'wan'         # Die Zone heisst wan
           list network 'wan'        # Die Netzwerke wan und wan6
                                     # gehören der Zone an
           list network 'wan6'       # Netzwerke werden definiert in
                                     # :file:`/etc/config/network`
           option input 'REJECT'     # Eingehenden Verkehr ablehnen
           option output 'ACCEPT'    # Ausgehenden Verkehr akzeptieren
           option forward 'REJECT'   # Eingehenden Verkehr nicht
                                     # weiterleiten
           option masq '1'           # NAT (Masquerading) für weiter-
                                     # geleiteten ausgehenden Verkehr
           option mtu_fix '1'        # MSS clamping Regeln für
                                     # ausgehenden Verkehr
                                     # (gegen MTU-Probleme)
           option local_restrict '1' # siehe Bemerkung unten

.. note::

   Die Option ``local_restrict`` ist keine Option die OpenWrt von
   Haus aus kennt. Sie ist freifunkspezifisch und sorgt dafür,
   dass Regeln eingefügt werden, die den Zugriff aufs Heimnetzwerk
   verbieten wenn der Freifunkrouter mit seiner WAN-Buchse am eigenen
   LAN-Netzwerk angeschlossen ist, siehe: :ref:`net-setup-standard`.

Eine Konfiguration über :command:`uci` ist zwar prinzipiell möglich,
jedoch umständlich da die Sektionen in der Konfigurationsdatei keine Namen
haben.

Nach Änderungen an der Firewall Konfiguration ist es notwendig die Firewall
durch das Kommando :command:`fw3 reload` neu zu starten.


.. _firewall-port-forward:

Port Forwarding
---------------

Port Forwarding wird eingesetzt, um einzelne Ports auf Rechnern hinter
dem Router zu öffnen. Dies ist z.B notwendig, wenn Ports auf einem Rechner
im LAN-Netzwerk aus dem Internet erreichbar sein sollen.

Das Ganze erklärt sich am besten durch ein Beispiel:

Das Setup ist wie in :ref:`net-setup-internetgw` beschrieben. Der
Freifunkrouter ist mit seiner WAN-Buchse direkt am Internet angeschlossen.
An einer der LAN-Buchsen ist ein Rechner mit der IP 192.168.1.249
angeschlossen, auf dem ein Webserver auf Port 80 läuft. Dieser Webserver
soll nun auch aus dem Internet erreichbar sein, und zwar unter Port 8080.

Das Portforwarding kann nun entweder über LuCI oder die Shell eingerichtet
werden.

Portforwarding mit LuCI einrichten
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Gehe zu :menuselection:`Administration --> Netzwerk --> Firewall --> Portweiterleitungen`.
Dort kannst du eine neue Portweiterleitung einrichten:

.. image:: /images/luci/firewall-portforward1.jpg

Gib die Daten wie oben gezeigt ein. Erklärung der einzelnen Optionen:

.. tabularcolumns:: |p{2.5cm}|p{8cm}|p{4.5cm}|

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Option
     - Beschreibung
     - Default
   * - Name
     - Ein Name für diese Port Forwarding Regel, hier web
     - keiner
   * - Protokoll
     - Nur Pakete von diesem Protokolltyp forwarden, hier TCP+UDP
       Es hätte auch TCP alleine gereicht, da HTTP nur TCP verwendet.
     - TCP+UDP
   * - Externe Zone
     - Firewallzone auf der den Router die Anfrage erreicht, hier ``wan``
     -
   * - Externer Port
     - Dieser Port soll weitergeleitet werden, hier ``8080``
     -
   * - Interne Zone
     - Die Firewallzone in der sich der Zielrechner der Weiterleitung befindet,
       hier ``lan``
     -
   * - Interner Port
     - An diesen Internen Port des Zielrechners werden Pakete weitergeleitet,
       hier ``80``
     - wie Externer Port

Nachdem alle Optionen ausgefüllt wurden klicke auf den :guilabel:`Speichern & Anwenden`
Button. Die Weiterleitung wird jetzt gespeichert, die Firewall neu geladen damit die
Weiterleitung aktiv wird und die Seite des Webinterfaces neu geladen.

Die Seite sieht mit der neuen Portweiterleitung nun so aus:
     
.. image:: /images/luci/firewall-portforward2.jpg
   
Es gibt hier nun auch die Möglichkeit, diese Regel zu (De-)aktivieren sowie bei
mehreren Weiterleitungsregeln die Reihenfolge der Regeln zu ändern. Mit einem
Klick auf :guilabel:`Bearbeiten` kann die Regel bearbeitet, mit einem Klick
auf :guilabel:`Löschen` gelöscht werden.

Portweiterleitung auf der Shell
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Die Weiterleitung lässt sich auch auf der Shell anlegen. Entweder man
editiert :file:`/etc/config/firewall` direkt und fügt folgende Sektion
hinzu:

.. code-block:: sh

   config redirect web
   	   option target 'DNAT'
	   option src 'wan'
	   option dest 'lan'
	   option proto 'tcp udp'
 	   option src_dport '8080'
	   option dest_ip '192.168.1.249'
	   option dest_port '80'
	   option name 'web'

oder direkt über :command:`uci` Kommandos:

.. code-block:: sh

   uci set firewall.web=redirect
   uci set firewall.web.target=DNAT
   uci set firewall.web.src=wan
   uci set firewall.web.dest=lan
   uci set firewall.web.proto='tcp udp'
   uci set firewall.web.src_dport=8080
   uci set firewall.web.dest_ip=192.168.1.249
   uci set firewall.web.dest_port=80
   uci set firewall.web.name=web
   uci commit firewall

In beiden Fällen muss die Konfiguration der Firewall durch
:command:`fw3 reload` neu eingelesen werden.
