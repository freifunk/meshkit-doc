.. include:: /links.txt

Bevors los geht: Vorbereitungen
===============================

.. _`Infos zum Router sammeln`:

Wichtige Infos zum Router sammeln
---------------------------------

Um eine passende Firmware für das eigene Routermodell zu generieren muss man
zunächst einige Daten zum Router kennen:

* Chipsatz
* Chipsatz des/der WLAN-Interfaces
* Größe des RAM
* Größe des Flash-Speichers

Diese Daten finden man in der `Table of Hardware`_ im OpenWrt Wiki. Zu den
meisten Routermodellen gibt es auch eine Detailseite mit weiteren Infos. Diese
Seite sollte man ebenfalls zumindest kurz überfliegen. Hier finden sich
im Allgemeinen auch Infos zum Flashen und zur Rettung des Routers falls beim 
Flashen etwas so richtig schief ging.

Beispiel:

Wir wollen die Daten für einen TP-Link WR1043ND zusammentragen. In der
`Table of Hardware`_ sehen wir folgendes:

.. image:: /images/toh-wr1043nd.jpg

Daraus können wir entnehmen:

* Der Chipsatz gehört zur AR71XX Familie
* WLAN benutzt Atheros Hardware
* Der RAM ist 32 MB groß
* Flash ist 8 MB groß

.. _`IP-registrieren`:

IP-Adresse(n) registrieren
--------------------------

Nun muss noch mindestens eine IP-Adresse aus dem Freifunknetz für den Router
registriert werden. Hier hat jede Community ihre eigenen Seiten zur Registrierung,
im Allgemeinen kann man sich hier jedoch selbst bedienen und einfach eine noch
nicht vergebene Adresse für seinen Knoten reservieren.

Registrierungsseiten in den einzelnen Communities:

* **Augsburg:** Als Benutzer auf http://augsburg.freifunk.net einloggen und "Node registrieren"

