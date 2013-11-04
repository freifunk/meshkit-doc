.. _integrate_other_pcs:


Angeschlossene Computer im Freifunknetz erreichbar machen
=========================================================

Sind eigene Computer über die LAN oder die WAN-Schnittstelle mit dem Router
verbunden (siehe :ref:`net-setup`), dann sind diese aus dem Freifunknetz
heraus nicht direkt erreichbar. Möchte man auf diesen nun Dienste (siehe:
:ref:`offer-services` anbieten, auf die aus dem Freifunknetz
zugegriffen werden kann, dann muss der Freifunkrouter entsprechend
umkonfiguriert werden.

.. _integrate_portfw:

Einzelne Dienste mit Portforwarding verfügbar machen
----------------------------------------------------

Das ist der einfachste Weg, der keine Eingriffe ins Netzwerk an sich
erfordert. Es wird eine (oder auch mehrere) Regel für Portforwarding
erstellt. Portforwarding funktioniert dabei so, dass Zugriffe auf den
Port einer externen Adresse auf einen Rechner hinter dem Freifunkrouter
weitergeleitet werden.

Zum Einrichten von Portforwards siehe :ref:`firewall-port-forward`.
Als ``Externe Zone`` ist in diesem Fall ``Freifunk`` zu wählen,
schliesslich sollen ja Pakete die an einen Port der Freifunk-
Schnittstelle(n) gerichtet sind weitergeleitet werden.

.. _integrate_hna:

Rechner oder Netzwerke mit HNA ankündigen
-----------------------------------------

Mit :term:`HNA` (Host Network Announcement) kann auf dem Node ein verfügbarer
IP-Bereich oder auch nur eine einzelne Adresse im Netzwerk bekannt gemacht
werden. Dadurch wird ein angeschlossener Rechner direkt im Mesh erreichbar,
muss jedoch selbst nicht :term:`OLSR` nutzen.

Um einen Rechner im WAN oder LAN (siehe :ref:`net-setup`) per :term:`HNA`
erreichbar zu machen, benötigt man ein eigenes kleines Netzwerk aus dem
Bereich der eigenen Freifunk Community und sollte dieses Netzwerk (bzw. darin
liegende einzelne IPs registrieren, siehe :ref:`IP-registrieren`. Soll nur
ein einzelner Rechner per HNA angekündigt werden, dann reicht es ein Netzwerk
mit der :term:`Netzmaske` /30 zu registrieren. Sollen es mehr Rechner werden dann dieses
Netz entsprechend grösser wählen (z.B. /29 für 6 Rechner oder /28 für 14 Rechner).

.. hint::

   Um Rechner über HNA erreichbar zu machen müssen die IP-Adressen auf dem
   Freifunkrouter **und** auf dem Rechner angepasst werden.

**Beispiel**

Im Augsburger Freifunknetz, das als Mesh-Netzwerk 10.11.0.0/18
benutzt soll ein am LAN des Freifunkrouters angeschlossener Rechner per HNA
verfügbar gemacht werden. Dazu wird zunächst das Netzwerk 10.11.9.0/30, also die
IPs 10.11.9.0 bis 10.11.9.3 reserviert. Das ergibt 2 nutzbare IPs:

* 10.11.9.1 soll als Alias-IP (also zusätzliche IP) für LAN auf dem
  Freifunkrouter verwendet werden
* 10.11.9.2 soll die IP-Adresse des angeschlossenen Rechners werden.

Einrichtung eines HNA-Netzwerks in LuCI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Gehe zu :menuselection:`Administration --> Netzwerk --> Schnittstellen`
und klicke unten auf :guilabel:`Neue Schnittstelle hinzufügen...`. Es öffnet
sich nun eine Seite zum konfigurieren der neuen Schnittstelle:

.. image:: /images/luci/hna-create-alias.jpg
   :alt: Alias Interface für die LAN Schnittstelle anlegen

Trage dort bei :guilabel:`Name der neuen Schnittstelle` (1) einen Namen ein.
Da wir hier ein HNA-Alias-Interface für LAN anlegen wollen verwenden wir
als Namen ``lanhna``. Wähle unten bei 
:guilabel:`Die folgende Schnittstelle abdecken`
:guilabel:`Benutzerdefinierte Schnittstelle` aus und trage in das Feld hinten
``@lan`` ein, damit die neue Schnittstelle als Alias-Interface der
``LAN``-Schnittstelle erzeugt wird.

.. hint::

   Ist der Rechner statt an den ``LAN``-Buchsen am ``WAN`` des Freifunkrouters
   angeschlossen gibst du hier ``@wan`` statt ``@lan`` ein.

Klicke abschliessend auf :guilabel:`Absenden` um zu einer weiteren
Konfigurationsseite zu kommen:

.. image:: /images/luci/hna-aliasiface-general.jpg
   :alt: Tab Allgemeine Einstellungen für das Alias Interface

Trage hier bei :guilabel:`IPv4-Adresse` (1) eine Adresse ein, in unserem Beispiel
also 10.11.9.1. Bei :guilabel:`IPv4 Netzmaske` wird die :term:`Netzmaske` des
Netzwerks eingetragen. In unserem Beispiel wäre das ``255.255.255.252``.

Wechsle danach zum Tab :guilabel:`Firewall Einstellungen`:

.. image:: /images/luci/hna-aliasiface-firewall.jpg
   :alt: Tab Firewall Einstellungen für das Alias Interface

Wähle hier bei :guilabel:`Firewallzone anlegen / zuweisen` die ``Freifunk``
Zone aus und beende das Anlegen des Interfaces durch einen Klick auf
:guilabel:`Speichern & Anwenden`.

Als nächstes muss noch ein :term:`HNA`-Eintrag für :term:`OLSR` angelegt werden.
Gehe dafür zu
:menuselection:`Administration --> Dienste --> OLSR --> HNA-Ankündigungen` und
klicke dort in der Sektion :guilabel:`Hna4` auf
:guilabel:`Hinzufügen`. Trage hier bei :guilabel:`Netzwerkadresse` (1) die Startadresse
des Alias-Netzwerks ein, in unserem Beispiel 10.11.9.0. Bei
:guilabel:`Netzmaske` (2) trage die :term:`Netzmaske` dieses Netzwerks ein,
hier ist das 255.255.255.252.

.. image:: /images/luci/hna-olsr.jpg
   :alt: HNA4 in der OLSR Konfiguration anlegen

Klicke anschliessend auf :guilabel:`Speichern & Anwenden`.



Einrichtung eines HNA-Netzwerks auf der Shell
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**1. Alias Interface anlegen**

Zunächst muss ein :term:`Alias-Interface` angelegt werden. Dazu wird am Ende von
:file:`/etc/config/network` folgendes eingefügt:

.. code-block:: sh

   	config interface 'lanhna'
   	option proto 'static'
   	option ifname '@lan'
   	option ipaddr '10.11.9.1'
   	option netmask '255.255.255.252'

Alternativ kann das auch mit :command:`uci` erledigt werden:

.. code-block:: sh

   uci set network.lanhna='interface'
   uci set network.lanhna.proto='static'
   uci set network.lanhna.ifname='@lan'
   uci set network.lanhna.ipaddr='10.11.9.1'
   uci set network.lanhna.netmask='255.255.255.252'

**2. Alias Schnittstelle zur Firewallzone ``freifunk` hinzufügen**

In :file:`/etc/config/firewall` muss die ``freifunk``-Zone bearbeitet und
dort das eben angelegte Alias-Interface ``lanhna`` für ``option network``
hinzugefügt werden.

.. code-block:: sh

   config zone 'zone_freifunk'
   	option name 'freifunk'
   	option input 'REJECT'
   	option forward 'REJECT'
   	option output 'ACCEPT'
   	option masq '1'
   	list masq_src 'lan'
   	list masq_src 'wireless0dhcp'
   	list masq_src 'wireless0ahdhcp'
   	option network 'wireless0 wireless0dhcp lanhna'

Auch das ist wiederum mit :command:`uci` direkt möglich:

.. code-block:: sh

   net="$(uci get firewall.zone_freifunk.network)"
   uci set firewall.zone_freifunk.network="$network lanhna"
   uci commit firewall

.. warning::

   Bei uci müssen wir hier den Weg gehen, zunächst herauszufinden, welche
   Netze bereits zur Zone gehören (Die Variable ``network`` aus der ersten Zeile).

**3. HNA Eintrag in der OLSR-Konfiguration erstellen**

Um einen :term:`HNA`-Eintrag zu Erstellen muss am Ende von :file:`/etc/config/olsrd`
folgendes eingefügt werden:

.. code-block:: sh

   config Hna4 'lanhna'
   	option netaddr '10.11.9.0'
   	option netmask '255.255.255.252'

oder alternativ wieder mit :command:`uci`:

.. code-block:: sh

   uci set olsrd.lanhna='Hna4'
   uci set olsrd.lanhna.netaddr='10.11.9.0'
   uci olsrd.lanhna.netmask='255.255.255.252'
   uci commit olsrd

**4. Dienste neu starten**

.. code-block:: sh

   /etc/init.d/network restart
   /etc/init.d/olsrd restart

Konfiguration des angeschlossenen Rechners
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Der per :term:`HNA` angeschlossene Rechner muss eine IP-Adresse aus dem Bereich
verwenden, der per HNA angekündigt wird. Dies kann die primäre IP des Rechners, aber
auch eine Alias-IP sein. In unserem Beispiel wäre die IP des Rechners 10.11.9.2 und
die :term:`Netzmaske` 255.255.255.252.


.. _integrate_olsr:

Rechner der selbst OLSR benutzt
-------------------------------

Indem ein Rechner/Server selbst :term:`OLSR` "spricht" kann er direkt aus dem
Mesh erreicht werden.

Auf dem Freifunkrouter soll daher ein **Alias-Netz** eingerichtet werden, das auf
``LAN`` zusätzlich zur LAN-IP eine IP-Adresse aus dem IP-Bereich der jeweiligen
Community hat. Der anzuschliessende Rechner braucht ebenfalls eine IP aus diesem
IP-Bereich. Siehe :ref:`IP-registrieren`. Zusätzlich muss auf beiden, Freifunkrouter
und angeschlossener PC :term:`OLSR` für die jeweilige Schnittstelle eingerichtet werden.

.. hint::

   Wenn du schon vorher weisst dass entweder per ``LAN`` oder ``WAN`` ein oder
   mehrere Rechner per OLSR angeschlossen werden sollen, dann kannst du auch
   im Meshkit beim erstellen des Images direkt OLSR als Protokoll für dieses
   Netzwerk wählen. Das betreffende Netzwerk wird dann direkt für OLSR-Betrieb
   konfiguriert. **Es gibt dann kein Alias, d.h. die urprüngliche Funktionalität
   wird nicht erhalten.** Siehe: :ref:`generate-expert-lan` bzw.
   :ref:`generate-expert-wan`.

**Beispiel für diese Konfiguration**

Der Freifunkrouter soll die IP 10.11.9.100 bekommen, der Rechner die IP 10.11.9.101.
Als :term:`Netzmaske` wird /18, also 255.255.192.0 verwendet.


Einrichtung eine Alias-Schnittstelle für OLSR unter LuCI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Gehe zu :menuselection:`Administration --> Netzwerk --> Schnittstellen`
und klicke unten auf :guilabel:`Neue Schnittstelle hinzufügen...`. Es öffnet
sich nun eine Seite zum konfigurieren der neuen Schnittstelle:

.. image:: /images/luci/alias-lan-olsr.png
   :alt: Alias Interface für die LAN Schnittstelle anlegen

Trage dort bei :guilabel:`Name der neuen Schnittstelle` (1) einen Namen ein.
Da wir hier ein OLSR-Alias-Interface für LAN anlegen wollen verwenden wir
als Namen ``lanolsr``. Wähle unten bei 
:guilabel:`Die folgende Schnittstelle abdecken`
:guilabel:`Benutzerdefinierte Schnittstelle` aus und trage in das Feld hinten
``@lan`` ein, damit die neue Schnittstelle als Alias-Interface der
``LAN``-Schnittstelle erzeugt wird.

.. hint::

   Ist der Rechner statt an den ``LAN``-Buchsen am ``WAN`` des Freifunkrouters
   angeschlossen gibst du hier ``@wan`` statt ``@lan`` ein.

Klicke abschliessend auf :guilabel:`Absenden` um zu einer weiteren
Konfigurationsseite zu kommen:

.. image:: /images/luci/alias-lan-basic-settings.png
   :alt: Tab Allgemeine Einstellungen für das Alias Interface

Trage hier bei :guilabel:`IPv4-Adresse` (1) eine Adresse ein, in unserem Beispiel
also 10.11.9.100. Bei :guilabel:`IPv4 Netzmaske` wird die :term:`Netzmaske` des
Netzwerks eingetragen. In unserem Beispiel wäre das ``255.255.192.0``.

Wechsle danach zum Tab :guilabel:`Firewall Einstellungen`:

.. image:: /images/luci/alias-lan-firewall.png
   :alt: Tab Firewall Einstellungen für das Alias Interface

Wähle hier bei :guilabel:`Firewallzone anlegen / zuweisen` die ``Freifunk``
Zone aus und beende das Anlegen des Interfaces durch einen Klick auf
:guilabel:`Speichern & Anwenden`.

Jetzt muss noch :term:`OLSR` für diese neu angelegte Schnittstelle
aktiviert werden. Verwende dabei ``lanolsr`` als Netzwerk. Siehe:
:ref:`olsr-iface-add`.


Einrichtung eine Alias-Schnittstelle für OLSR auf der Shell
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**1. Alias Interface anlegen**

Zunächst muss ein :term:`Alias-Interface` angelegt werden. Dazu wird am Ende von
:file:`/etc/config/network` folgendes eingefügt:

.. code-block:: sh

   	config interface 'lanolsr'
   	option proto 'static'
   	option ifname '@lan'
   	option ipaddr '10.11.9.100'
   	option netmask '255.255.192.0'


Alternativ kann das auch mit :command:`uci` erledigt werden:

.. code-block:: sh

   uci set network.lanolsr='interface'
   uci set network.lanolsr.proto='static'
   uci set network.lanolsr.ifname='@lan'
   uci set network.lanolsr.ipaddr='10.11.9.100'
   uci set network.lanolsr.netmask='255.255.192.0'

**2. Alias Schnittstelle zur Firewallzone ``freifunk` hinzufügen**

In :file:`/etc/config/firewall` muss die ``freifunk``-Zone bearbeitet und
dort das eben angelegte Alias-Interface ``lanolsr`` für ``option network``
hinzugefügt werden.

.. code-block:: sh

   config zone 'zone_freifunk'
   	option name 'freifunk'
   	option input 'REJECT'
   	option forward 'REJECT'
   	option output 'ACCEPT'
   	option masq '1'
   	list masq_src 'lan'
   	list masq_src 'wireless0dhcp'
   	list masq_src 'wireless0ahdhcp'
   	option network 'wireless0 wireless0dhcp lanolsr'

Auch das ist wiederum mit :command:`uci` direkt möglich:

.. code-block:: sh

   net="$(uci get firewall.zone_freifunk.network)"
   uci set firewall.zone_freifunk.network="$network lanolsr"
   uci commit firewall

.. warning::

   Bei uci müssen wir hier den Weg gehen, zunächst herauszufinden, welche
   Netze bereits zur Zone gehören (Die Variable ``network`` aus der ersten Zeile).

**3. OLSR für das Interface aktivieren**

Jetzt muss noch :term:`OLSR` für diese neu angelegte Schnittstelle
aktiviert werden. Verwende dabei ``lanolsr`` als Netzwerk. Siehe:
:ref:`olsr-iface-add`.

**4. Dienste neu starten**

.. code-block:: sh

   /etc/init.d/network restart
   /etc/init.d/olsrd restart

Konfiguration des angeschlossenen Rechners
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Der angeschlossene Rechner muss eine IP aus dem Mesh-Netzwerk der
eigenen Community verwenden. Siehe :ref:`IP-registrieren`.

Zudem muss `OLSR` auf dem Rechner konfiguriert und gestartet werden.

.. hint::

   Auf dem Freifunkrouter findet man auf der Seite
   :menuselection:`Freifunk --> OLSR` unter
   :ref:`Konfiguration herunterladen <olsr-download-config>` die auf dem
   Router aktive OLSR Konfiguration. Diese kann sehr gut  als Ausgangsbasis
   für die OLSR-Konfiguration auf dem Rechner verwendet werden.
 
