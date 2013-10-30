Grundeinstellungen
==================

Zu finden unter :menuselection:`Administration --> Freifunk --> Grundeinstellungen`

Community
---------

Wählt die Community. Dadurch werden Default-Werte für den Meshwizard
vorgegeben. In der Regel gibt es hier nichts auszuwählen. Soll ein
Router in einer anderen Community neu in Betrieb genommen werden, dann
ist es sinnvoller, für diesen ein neues Image mit Meshkit zu erstellen
und den Router neu zu flashen.

Will man es dennoch ohne neu zu flashen versuchen, dann kann man mit

.. code-block:: sh

   opkg update
   opkg install community-profiles

alle weiteren existierenden Community-Profile installieren.

Siehe auch: :ref:`community_profiles`

.. image:: /images/luci/freifunk-grundeinstellungen-community.png

Grundlegende Systemeinstellungen
--------------------------------

Hier können der **Name des Routers**, der **Standort** sowie die **Geokoordinaten**
geändert werden. Ein Klick auf :guilabel:`OpenStreetmap anzeigen` öffnet
eine Karte, in der die Geokoordinaten bequem ausgewählt werden können.

.. image:: /images/luci/freifunk-grundeinstellungen-grundlegende.png

