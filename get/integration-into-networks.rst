.. include:: /links.txt

.. _net-setup:

Einbinden des Freifunkrouters ins eigene Netzwerk
=================================================

In diesem Kapitel soll es draum gehen, wie ein Freifunk Router ins eigene
Netzwerk integriert werden kann. Je nach bestehender Netzwerktopologie und
Anforderungen kann der Freifunk Router auf unterschiedliche Art integriert
werden.

.. important::
   Bevor die Meshkit Firmware generiert werden kann ist es wichtig im voraus zu
   wissen, wie der Freifunk Router ins eigene Netzwerk integriert werden soll.

LAN, WAN und Freifunk - Die einzelnen Zonen im Router
-----------------------------------------------------

.. image:: /images/wifi-router-back.jpg
   :width: 300px
   :alt: Typische Netzwerke im Freifunkrouter
   :align: right

Bevor einzelne Integrationsvarianten besprochen werden ist es wichtig zu verstehen,
dass der Freifunk Router in der Standardkonfiguration drei Netzwerke kennt. Jedes
dieser drei Netzwerke erfüllt unterschiedliche Aufgaben und es gelten unterschiedliche
Regeln, vor allem im Hinblick auf die Konfiguration der Firewall.

Nicht jeder Router verfügt über genug Schnittstellen um alle drei Zonen zur Verfügung zu stellen.
Gibt es z.B. nur eine Ethernet-Buchse am Router dann wird durch OpenWrt vorgegeben, welchem
Netzwerk diese Buchse angehört. In der Regel konfiguriert OpenWrt diese jedoch dann als LAN.
Genaueres dazu findest du in der `Table of Hardware`_ im OpenWrt Wiki.

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Zone
     - Aufgabe
     - Default IP
   * - LAN
     - Dies ist das lokale Netzwerk (Local Area Network). Hier werden eigene PCs angeschlossen.
     - 192.168.1.1
   * - WAN
     - Wide Area Network. Damit verbindet man den Router mit dem Internet.
     - automatisch (DHCP)
   * - Freifunk
     - Für die Verbindung zum Freifunknetz
     - individuell

Für die einzelnen Zonen gelten spezifische **Firewallregeln**, die bestimmen, ob
Verkehr der den Router über diese Zonen erreicht akzeptiert (und evtl. weitergeleitet)
ode verworfen wird, siehe dazu: :ref:`firewall-zones`.

.. _net-setup-standard:

Der Standardfall: Freifunk Router am Heimnetzwerk
-------------------------------------------------

.. image:: /images/net-setup-standard.jpg
   :alt: Standardsetup - Freifunkrouter als Teil des LAN
   :width: 40%
   :align: right

Der Freifunkrouter ist mit seiner WAN-Buchse am LAN des lokalen Router/Switch angeschlossen
und damit Teil des eigenen Netzwerks. Eigene PCs sind ebenfalls mit diesem Router/Switch
verbunden.

* PC und Freifunk Router sind im selben Netzwerk (LAN)
* Beide beziehen in der Regel automatisch eine IP-Adresse vom eigenen Router (DHCP)
* Der eigene Router dient als Internetgateway
* Ob auch Freifunknutzer den eigenen Internetzugang benutzen dürfen kann im Meshkit
  mit der Option ``Internet freigeben`` (siehe :ref:`generate-expert-wan`) konfiguriert werden.
* Auf Rechner im LAN kann von Freifunk aus nicht direkt zugegriffen werden. Will man Dienste
  auf Rechnern im LAN verfügbar machen muss man dies auf dem Freifunkrouter konfigurieren (TODO: Link zur Anleitung)
* Um vom WAN aus auf den Freifunk Router zugreifen zu können, muss die Firewall dort
  konfiguriert werden. Das kann man sich einfach machen indem man beim meshkit im ``WAN`` Tab
  Häckchen bei ``Erlaube SSH`` und ``Erlaube Web`` setzt (siehe :ref:`generate-expert-wan`)
* Will man vom eigenen LAN aus auf das Freifunknetz zugreifen können, dann müssen entweder
  auf dem eigenen Rechner oder besser auf dem eigenen Gateway statische Routen eingetragen
  und die Firewall auf dem Freifunk Router entsprechend konfiguriert werden (TODO) werden.


.. _net-setup-internetgw:

Freifunk Router direkt am Internetzugang
----------------------------------------

.. image:: /images/net-setup-internetgw.jpg
   :alt: Freifunkrouter als Internetgateway
   :width: 40%
   :align: right

Der Router ist mit seinem WAN-Port direkt am Internet (z.B. einem DSL-Modem)
angeschlossen und stellt die Verbindung zum Internet selbst her. Eigene PCs
werden an den LAN-Ports des Routers angeschlossen oder verbinden sich per
WLAN. Auf diese Weise kann der Freifunk Router unter Umständen auch ein
bereits vorhandenes Gerät ersetzen.

* Auf Rechner im LAN kann von Freifunk aus nicht direkt zugegriffen werden. Will man Dienste
  auf Rechnern im LAN verfügbar machen muss man dies auf dem Freifunkrouter konfigurieren (TODO: Link zur Anleitung)
* Rechner die am LAN angeschlossen sind können andere Adressen im Freifunknetzwerk
  direkt erreichen
* Wenn die Hardware des Freifunkrouters das Erstellen von Virtuellen Access
  Points (VAP) erlaubt, dann kann zusätzlich zum Freifunknetz ein weiteres
  verschlüsseltes WLAN-Netz für die eigene Benutzung geöffnet werden. (TODO: Anleitung)

.. hint::
   Viele Provider benutzen VOIP (SIP) zum Bereitstellen von Telefoniefunktionen.
   Dabei kümmert sich der vom Provider zur Verfügung gestellte Router um die VOIP-
   Verbindungen. Ist dies bei dir der Fall, dann ist dieses Setup eher nichts für
   dich und es ist wahrscheinlich sinnvoller, nach :ref:`net-setup-standard`
   vorzugehen.


.. container:: clearer

   .. no content here, this is just to clear the float from previous image

.. _net-setup-ffonly:


Freifunk Router ohne eigenen Internetzugang
-------------------------------------------

.. image:: /images/net-setup-ffonly.png
   :alt: Freifunkrouter ohne eigenen Internetzugang
   :width: 40%
   :align: right

Ein Freifunkrouter lässt sich auch ohne Anschluss an einen Internetzugang
bzw. ans eigene Heimnetz betreiben. Eigene Rechner sind in diesem Fall an
den LAN-Schnittstellen des Freifunkrouters angeschlossen. Voraussetzung ist,
dass über Funk eine Verbindung zum restlichen Teil des Freifunknetzwerks
möglich ist, sich also mindestens ein Nachbar in Funkreichweite befindet. Ist
dies der Fall, dann wird über diese Funkverbindung Kontakt zum Freifunknetzwerk
hergestellt. Damit ist dann in der Regel auch der Zugriff aufs Internet vom
Router selbst und angeschlossenen Rechnern möglich.

* Auf Rechner im LAN kann von Freifunk aus nicht direkt zugegriffen werden. Will man Dienste
  auf Rechnern im LAN verfügbar machen muss man dies auf dem Freifunkrouter konfigurieren (TODO: Link zur Anleitung)
* Rechner die am LAN angeschlossen sind können andere Adressen im Freifunknetzwerk
  direkt erreichen
