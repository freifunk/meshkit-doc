.. include:: /links.txt

Allgemeine Infos zur Meshkit Freifunk Firmware
==============================================

Was ist Meshkit?
----------------

Meshkit ist ein Generator für individualisierte Freifunk-Firmware-Images, die direkt
nach dem Flashen einsatzbereit sind. Dazu gibt man im `Meshkit`_ ein paar Daten ein
(IP-Adresse, Standort, Kontaktdaten). Meshkit generiert dann ein Firmwareimage (das
Betriebsystem des Freifunk Routers) mit diesen Einstellungen. Dieses Image kann
dann auf einen kompatiblem Access Point geflasht werden um so Teil des Freifunknetzes
zu werden.

Features
--------

* Support für Communityprofile (verschiedene Default-Einstellungen für unterschiedliche Communities)
* Erstellen vorkonfigurierter Firmwareimages, die direkt nach dem Flashen im Mesh erreichbar sind.
* Pakete können direkt mit ins Image gebaut werden (das spart Platz dank besserer Kopmression)
* Eigene Dateien können mit ins Image gebaut werden

Unterstützte Routermodelle
--------------------------

Prinzipiell werden alle Router unterstützt, die von der von Meshkit verwendeten
OpenWrt-Version unterstützt werden und für die es ein ``Target`` gibt. Es werden nur
Router mit **mindestens 32MB Ram** unterstützt. Was die **Flash-Grösse** anbeglangt so
sind 4MB gerade noch genug, empfohlen werden aber mindestens **8MB Flash**. Informationen
zu den einzelnen von OpenWrt unterstützten Geräten (Target, Ram, Flash etc.)
gibt es in der OpenWrt `Table of Hardware`_.

Im Moment werden von `Meshkit`_ die folgenden Targets unterstützt:

* ar71xx
* brcm47xx
* brcm63xx
* x86
* x86-kvm


Repository
----------

Der Quellcode von Meshkit (GPL-Lizenz) befindet sich auf
`Github <http://www.github.com/freifunk/meshkit>`_. 

Diese Dokumentation ist ebenfalls auf Github verfügbar:
`Meshkit Dokumentation <http://www.github.com/freifunk/meshkit-doc>`_.

Für Meshkit und die Meshkit Dokumentation gibt es ebenfalls auf Github einen
Bugtracker ("Issues").

Von Meshkit benutzte Software
-----------------------------

* Die Meshkit Weboberfläche baut auf `web2py <http://web2py.com/>`_ auf
* Die Firmware basiert auf einem möglichst wenig angepassten `OpenWrt`_
* Als Weboberfläche wird `LuCI`_ verwendet
* Als Routingprotokoll kommt `OLSRd`_ zum Einsatz

