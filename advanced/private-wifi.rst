.. _private-wifi:

Privates WLAN-Netzwerk einrichten
=================================

Wenn die WLAN-Hardware des Routers die Einrichtung von :term:`VAP`
(Virtuellen Access Points) unterstützt, dann kann zusätzlich zu dem/den
Freifunknetz(en) ein **privates WLAN** eingerichtet werden, das
Verschlüsselung benutzt und Teil des ``LAN``-Netzwerks (siehe :ref:`net-zones`)
ist.

.. hint::

   Soll Verschlüsselung benutzt werden dann muss das Paket ``hostapd-mini``
   installiert werden. Zum Installieren von Paketen siehe
   :ref:`packages`.

Privates Netzwerk mit LuCI einrichten
-------------------------------------

Es muss ein weiteres WLAN-Gerät erstellt und dem Netzwerk ``LAN`` zugeordnet
werden.

Gehe dafür zu :menuselection:`Administration --> Netzwerk --> Drahtlos` wo du
eine Übersicht über die WLAN-Netzwerke siehst:

.. image:: /images/luci/add-wifi-interface.png
   :alt: Ein Wifi Interface in LuCI hinzufügen

Klicke dort auf :guilabel:`Hinzufügen` (1). Es öffnet sich ein weiteres
Fenster mit den Einstellungen für das neue Interface:

.. image:: /images/luci/add-private-wifi-detail.png
   :alt: Detaileinstellungen privates Wifi Interface in LuCI

Stelle bei :guilabel:`Kanal` (1) zunächst sicher, dass der selbe Kanal wie
für die andere(n) SChnittstelle(n) verwendet wird. **Mehrere VAPs können nur
auf einem gemeinsamen Kanal betrieben werden.** Ändere anschliessend die
:guilabel:`ESSID` (2) des Netzwerks. Wähle aus, dass das Wifi-Interface zum Netzwerk
``LAN`` (3) gehören soll.

Soll das Netzwerk verschlüsselt werden (empfehlenswert!), dann anschliessend noch zum
Tab :guilabel:`Verschlüsselung` (4) wechseln und folgende Einstellungen vornehmen:

.. image:: /images/luci/add-private-wifi-crypto.png
   :alt: Verschlüsselungseinstellungen privates Wifi Interface in LuCI

Wähle als Verschlüsselungsmethode :guilabel:`WPA-PSK`, :guilabel:`WPA2-PSK`
oder :guilabel:`WPA-PSK/WPA2-PSK Mixed Mode` (1). Gib anschliessend bei
:guilabel:`Schlüssel` (2) ein ausreichend sicheres Passwort ein.

Nachdem die Einstellungen gemacht wurden, klicke auf
:guilabel:`Speichern & Anwenden` um die Einstellungen zu übernehmen.

Du kannst dich nun mit deinem Computer oder Smartphone mit dem verschlüsselten
WLAN verbinden.

Privates WLAN auf der Shell einrichten
--------------------------------------

Es muss die Konfigurationsdatei :file:`/etc/config/wireless` bearbeitet werden.
Hier wird amd Ende folgendes eingefügt::

  config wifi-iface 'radio0_iface_private' 
  	option device 'radio0'
  	option mode 'ap'
  	option ssid 'MeinPrivatesWLAN'
  	option network 'lan'
  	option encryption 'psk2'
  	option key 'xxx'

Ersetze hierbei wenn notwendig ``radio0`` mit dem Namen des wifi-device (siehe
in der Konfiguration weiter oben im selben File).

Die selben Einstellungen können auch mit :command:`uci` gemacht werden::

  uci set wireless.radio0_iface_private=wifi-iface
  uci set wireless.radio0_iface_private.device=radio0
  uci set wireless.radio0_iface_private.mode=ap
  uci set wireless.radio0_iface_private.ssid='MeinPrivatesWLAN'
  uci set wireless.radio0_iface_private.network=lan
  uci set wireless.radio0_iface_private.encryption=psk2
  uci set wireless.radio0_iface_private.key='geheimgeheim'
  uci commit wireless

Anschliessend müssen die WLAN-Interfaces neu gestartet werden, dies kann
durch Eingabe von :command:`wifi` erledigt werden.
