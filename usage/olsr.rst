.. include:: /links.txt

.. _olsr:

OLSR einrichten und konfigurieren
=================================

:term:`OLSR` ist der Routingdämon der dafür sorgt, das Pakete im Mesh
immer den richtigen Weg nehmen.

.. _olsr-iface-add:

Ein Interface für OLSR konfigurieren
------------------------------------

Hier wird gezeigt, wie man OLSR für eine Netzwerkschnittstelle in der
Meshkit Firmware aktiviert.

.. warning::

   Hier wird nur gezeigt wie man OLSR für eine Schnittstelle aktiviert.
   Die Schnittstelle selbst muss auch konfiguriert werden und der
   ``Freifunk``-Firewallzone hinzugefügt werden.


OLSR für eine Schnittstelle mit LuCI einrichten
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Gehe zu :menuselection:`Administration --> Dienste --> OLSR`. Ganz
unten siehst du die :guilabel:`Schnittstellen` Konfiguration:

.. image:: /images/luci/olsr-iface-add.png
   :alt: OLSR Interfaces Übersicht

Klicke dort auf :guilabel:`Hinzufügen`. Es öffnet sich eine neue
Seite, um das hinzugefügte Interface zu konfigurieren:

.. image:: /images/luci/olsr-iface-add-detail.png
   :alt: OLSR Interface Konfiguration

Wähle dort bei :guilabel:`Netzwerk` das Netzwerk aus, für das
:term:`OLSR` konfiguriert werden soll. In diesem Beispiel
wählen wir ``lanolsr``, das in :ref:`integrate_olsr` angelegt wurde.

Alle anderen Einstellungen können in der Regel auf ihren
Defaultwerten gelassen werden. Speichere die konfiguration und starte
olsrd neu durch einen Klick auf :guilabel:`Speichern & Anwenden`.


OLSR für eine Schnittstelle auf der Shell einrichten
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Um OLSR für ein Netzwerk auf der Shell zu aktivieren fügt man am Ende
von :file:`/etc/config/olsrd` folgendes ein:

.. code-block:: sh

   config Interface 'lanolsr'
   	option ignore '0'
   	option interface 'lanolsr'
   	option Mode 'mesh'

Natürlich geht auch dies wieder mit :command:`uci`:

.. code-block:: sh

   uci set olsrd.lanolsr=Interface
   uci set olsrd.lanolsr.ignore=0
   uci set olsrd.lanolsr.interface='lanolsr'
   uci set olsrd.lanolsr.Mode='mesh'
   uci commit olsrd

.. note::

   Ersetze ``lanolsr`` im den Beispielen oben durch den richtigen Namen des
   Netzwerks. Dies ist **nicht** der Name der phsyikalischen Schnittstelle
   sondern der Name unter dem OpenWrt dieses Netzwerk kennt, z.B. ``lan``.

Anschliessend muss olsrd neu gestartet werden:

.. code-block:: sh

   /etc/init.d/olsrd restart
