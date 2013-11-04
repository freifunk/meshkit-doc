.. include:: /links.txt

.. _olsr-status:

OLSR Statusinformationen anzeigen
=================================

:term:`OLSR` ist der Routingdämon der dafür sorgt, das Pakete im Mesh
immer den richtigen Weg nehmen. Status zu OLSR kann sowohl im LuCI
Webinterface als auch auf der Shell angezeigt werden

OLSR-Statusinformationen in LuCI
--------------------------------

Statusinfos zu OLSR können sowohl im Administrationsmenu von LuCI
(:menuselection:`Administration --> Status --> OLSR`) als auch im
öffentlichen Teil des Webinterfaces (:menuselection:`Freifunk --> OLSR`)
angezeigt werden. Es werden jeweils die selben Informationen angezeigt.

OLSR-Übersicht
^^^^^^^^^^^^^^

Zeigt eine Übersicht über den OLSR-Status.

.. image:: /images/luci/olsr-status-overview.png
   :alt: OLSR Status Übersicht

In der oberen Sektion :guilabel:`Netzwerk` sind einige allgemeine Metriken zu OLSR zu sehen.
Durch einen Klick auf die jeweilige Zahl kommt man zur entsprechenden Detailseite.

.. tabularcolumns:: |p{5cm}|p{10cm}|

.. list-table::
   :widths: 33 67
   :header-rows: 1

   * - Metrik
     - Beschreibung
   * - Schniistellen
     - Anzahl der Schnittstellen die OLSR nutzt
   * - Nachbarn
     - Anzahl der direkten Nachbarn
   * - Knoten
     - Anzahl aller bekannten Knoten im Mesh
   * - HNA
     - Anzahl bekannter :term:`HNA` im Mesh
   * - Verbindungen insgesamt
     - Anzahl aller Links im Mesh
   * - Verbindungen pro Node
     - durchschnittliche Verbindungen pro Knoten

In der unteren Sektion :guilabel:`OLSR-Konfiguration` sind Infos zur OLSR-Version
sowie zur OLSR-Konfiguration zu finden. Es ist möglich, die auf dem Knoten laufende
Konfiguration in verschiedenen Formaten herunterzuladen:

.. _olsr-download-config:

* :guilabel:`OpenWrt` - Konfiguration für OpenWrt (:term:`UCI`)
* :guilabel:`OLSRD IPv4` - IPv4 Konfiguration für OLSRD
* :guilabel:`OLSRD IPv6` - IPv6 Konfiguration für OLSRD


OLSR-Statusinformationen auf der Shell
--------------------------------------

neigh.sh
^^^^^^^^

neigh.sh ist ein kleines Script das Informationen zu den direkten Nachbarn ausgibt, z.B.::

  root@freifunk:~# neigh.sh
  Local      Remote     vTime LQ       NLQ      Cost 
  10.11.0.18 10.11.0.17 38004 0.894000 1.000000 1145 
  10.11.0.18 10.11.0.8  37751 1.000000 0.894000 1145 
  
  Local               Remote              vTime LQ       NLQ      Cost 
  fdca:ffee:ffa:12::1 fdca:ffee:ffa:11::1 39178 1.000000 1.000000 1024 
  fdca:ffee:ffa:12::1 fdca:ffee:ffa:8::1  36777 1.000000 1.000000 1024

jsoninfo plugin direkt befragen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Auf allen Knoten läuft ``olsrd-jsoninfo``, das Informationen zu :term:`OLSR`
im :term:`JSON`-Format ausgibt. Um Informationen vom jsoninfo-Plugin zu bekommen
kann :command:`nc` verwendet werden, z.B. um alle Informationen von jsoninfo
zu bekommen::

  echo "/all" | nc 127.0.0.1 9090

Ist man nur an bestimmten Informationen können auch nur diese abgefragt werden,
z.B. um Informationen zu den Nachbarn zu bekommen::

  echo "/neighbors" | nc 127.0.0.1 9090

Will man diese Informationen für den IPv6-OLSR erhalten dann ersetzt man ``127.0.0.1``
mit ``::1``.

Folgende Detailinformationen können abgefragt werden:

.. tabularcolumns:: |p{5cm}|p{10cm}|

.. list-table::
   :widths: 33 67
   :header-rows: 1

   * - Schlüssel
     - Beschreibung
   * - all
     - Alle Informationen
   * - neighbors
     - Nachbarn (inklusive 2-hop Nachbarn)
   * - links
     - bestehende Links zu direkten Nachbarn
   * - routes
     - Alle im netzwerk bekannten Routen
   * - hna
     - Alle im Netzwerk bekannten :term:`HNA`
   * - mid
     - Alle im Netzwerk bekannten :term:`MID`
   * - gateways
     - Smart-Gateways im Netz (nur wenn das Smart Gateway Plugin aktiv ist)
   * - interfaces
     - Informationen zu Interfaces auf denen OLSR läuft
   * - status
     - Gibt Infos zu neighbors, links. routes, hna, mid, gateways und interfaces kombiniert aus
   * - config
     - aktuelle Konfiguration
   * - olsrd.conf
     - aktuelle Konfiguration im Format des :file:`olsrd.conf` Konfigurationsfiles
   * - plugins
     - Informationen zu geladenen Plugins
