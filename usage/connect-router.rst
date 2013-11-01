.. include:: /links.txt

.. _connect:

Mit dem Router verbinden
========================

Zu Diagnose- und Konfigurationszwecken kann man sich mit dem Freifunk
Router entweder über SSH oder einen Webbrowser verbinden.

Netzwerkverbindung herstellen
-----------------------------

Um auf den Router über Netzwerk zuzugreifen brauchst du zunächst eine
Netzwerkverbindung mit dem Router. Je nachdem wie dein PC/Notebook
mit dem Router verbunden ist (siehe :ref:`net-setup`) muss unterschiedlich
vorgegangen werden. zunächst muss die IP-Adresse des Routers herausgefunden
werden.

Standardsetup: Router mit WAN-Schnittstelle am eigenen LAN
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note::
   Dies bezieht sich auf einen Freifunk Router, der so ins eigene Netzwerk
   eingebunden ist: :ref:`net-setup-standard`

.. attention::
   Zugriffe über die WAN-Schnittstelle des Routers werden normalerweise
   von der Firewall verboten und müssen explizit erlaubt werden. Am einfachsten
   geht das indem man im Meshkit im WAN-Tab ``Zugriff über SSH erlauben`` und
   ``Zugriff über Web erlauben`` auswählt. Siehe: :ref:`generate-expert-wan`.

Bezieht der Freifunkrouter seine IP automatisch per :term:`DHCP` vom eigenen Router,
was die Standardeinstellung ist, dann muss zunächst herausgefunden werden, welche
IP der Freifunkrouter vom eigenen Router bekommen hat. Dein eigener Router hat
hoffentlich ein Webinterface, das irgendwo anzeigt, welche Clients verbunden sind
und welche IPs diese haben. Dort solltest du dann die IP des Freifunk Routers
finden.

Wurde beim generieren des Images für den Router im Meshkit eine statische
IP-Adresse vergeben, dann verwende die dort vergebene IP. Verbinde dich jetzt
mit dieser IP zum Freifunkrouter: :ref:`connect-router`

Freifunk Router direkt am Internet, Client am LAN
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note::
   Dies bezieht sich auf einen Freifunk Router, der direkt mit dem Internet verbunden
   und der eigene PC mit der LAN-Buchse des Freifunk Routers verbunden ist.
   Siehe: :ref:`net-setup-internetgw`

Ein PC, der direkt am LAN-Anschluss des Routers angeschlossen ist erhält von
diesem automatisch per :term:`DHCP` eine IP-Adresse zugewiesen.

Sofern die IP des LAN-Interfaces des Freifunk Routers nicht verändert wurde
ist die IP zum Zugriff auf den Router über die LAN-Schnittstelle immer
``192.168.1.1``.

Verbindung über das Freifunknetz (WLAN)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Verbindet man sich per WLAN mit dem Router (ESSID ``Freifunk-10.z.x.y`` oder
``stadt.freifunk.net`` erhält man automatisch eine IP vom Router.

Danach kann auch aus dem Freifunknetz kann auf den Router zugegriffen werden. Verwende
dazu die IP-Adresse, die du dem Router beim Generieren des Firmwareimages
mit dem Meshkit gegeben hast, z.B. 10.0.0.1. Der Router ist unter dieser Adresse
aus dem ganzen Freifunknetz erreichbar, d.h. man kann auch auf den Router zugreifen,
wenn man an anderer Stelle im Freifunknetz ist.


.. _connect-router:

Zugriff auf den Freifunk Router
-------------------------------

Es gibt zwei Wege sich mit dem Router zu verbinden, um Status abzufragen
oder die Konfiguration zu ändern. Entweder man öffnet in
einem Browser die Webseite des Routers oder verbindet sich per ssh. In beiden
Fällen ist das Passwort für den Login ``root`` und das Defaultpasswort ``admin``.

Zugang zum Webinterface
^^^^^^^^^^^^^^^^^^^^^^^

Auf dem Freifunk Router läuft das LuCI-Webinterface, das Statusinformationen
anzeigt und wo Einstellungen am Router vorgenommen werden können. Auf das
Webinterface kann mit jedem Browser über die Adresse

  http://<ip-adresse>

zugegriffen werden.

.. _connect-ssh:

Zugang per SSH
^^^^^^^^^^^^^^

mit SSH kann man direkt auf die Kommandozeile des Freifunk routers zugreifen
und dort mit den gängigen Linux Befehlen arbeiten. Erfahrene Nutzer erledigen
Aufgaben auf der Kommandozeile des Routers oft schneller als über das Webinterface.

Bei Linux ist ein SSH-Client meist schon installiert und man kann sich einfach mit

.. code-block:: sh

   ssh root@<ip-adresse>

von der Kommandozeile aus mit dem SSH-Server des Freifunkrouters verbinden.

Für Windows ist `Putty`_ eine häufig genutzte Anwendung zur Benutzung von SSH.
